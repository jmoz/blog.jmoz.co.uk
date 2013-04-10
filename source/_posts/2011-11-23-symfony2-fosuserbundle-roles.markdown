---
layout: post
title: "Symfony2 FOSUserBundle Roles - a simple solution"
date: 2011-11-23 12:00
comments: true
categories: [php, symfony2, fosuserbundle]
published: true
---
Out of the box, **FOSUserBundle** does NOT support Doctrine ORM database persisted **Roles**.  The base class they give you to extend has functionality for roles, i.e. setting, getting and checking them, but you will need to write your own code to persist to the database and thus fully equipping the User with role functionality.

The quickest and easiest way to get roles up and running is to use the existing FOS functionality and create your user a new roles field, mapping it as an array type in Doctrine.  This is quicker and easier to work with than using separate Role objects.  So your user class will need to specify the `$roles` class member with the following mapping metadata:

    /**
     * @ORM\Column(type="array", name="roles")
     */
     protected $roles;

This will tell doctrine to map a PHP array to a SQL CLOB using `serialize()` and `unserialize()`.  Take a look at the [Doctrine mapping documentation](http://www.doctrine-project.org/docs/orm/2.0/en/reference/basic-mapping.html#doctrine-mapping-types).

## Schema changes

So now the model has been sorted, the new column needs to be added to the database.  Generate a migration for the changes to the users table:

    $ php app/console doctrine:migrations:diff
    Generated new migration class to "/home/james/fooapp/DoctrineMigrations/Version20111121145741.php" from schema differences.

This will generate said file which will contain a call to a method to add the new column:

    $this->addSql("ALTER TABLE users ADD roles LONGTEXT NOT NULL COMMENT '(DC2Type:array)'");

If you want to set up default roles for existing users, under the call to `addSql()` insert the following line:

    $this->addSql(sprintf("UPDATE users SET roles = '%s'", 'a:1:{i:0;s:8:"ROLE_FOO";}'));

Then execute:

    $ php app/console doctrine:migrations:migrate
    ...
    Migrating up to 20111121145921 from 0

      ++ migrating 20111121145921

         -> ALTER TABLE users ADD roles LONGTEXT NOT NULL COMMENT '(DC2Type:array)'
         -> UPDATE users SET roles = 'a:1:{i:0;s:8:"ROLE_FOO";}'

      ++ migrated (0.69s)

      ------------------------

      ++ finished in 0.69
      ++ 1 migrations executed
      ++ 2 sql queries

More information on migrations can be found at [symfony](http://symfony.com/doc/2.0/bundles/DoctrineMigrationsBundle/index.html) and [github](https://github.com/symfony/DoctrineMigrationsBundle).

This will add the new column to your users table.  You now need to create a new user which will save a serialized empty array in the roles column.  From here you can add new roles to that user or you may need to sort out existing users which will have nothing in their roles field unless you followed the step above.  Trying to update or work with existing users with empty roles fields will most likely give you a `Doctrine ConversionException`:

    [Doctrine\DBAL\Types\ConversionException]                   
    Could not convert database value "" to Doctrine Type array

So I suggest setting up a new user with default roles for your system then copying the content of that user's roles field into existing user's roles fields.

To test the roles functionality try the following:

    $ php app/console fos:user:create jamestest james@foo.co.uk mypassword
    Created user jamestest
    $ php app/console fos:user:promote jamestest --super
    User "jamestest" has been promoted as a super administrator.

If you now check your db, the roles field for jamestest should look similar to:

    a:2:{i:0;s:8:"ROLE_FOO";i:1;s:16:"ROLE_SUPER_ADMIN";}

Your User should now be set up to persist it's roles to the database.  You should be able to edit the security.yml file and restrict access to certain parts of your site by url pattern:

    access_control:
      - { path: ^/admin, roles: ROLE_FOO }

For further information see [symfony security](http://symfony.com/doc/2.0/book/security.html#securing-specific-url-patterns).