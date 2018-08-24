# Manager

The manager provide a central way to create/add users, roles and permission.

```PHP
$manager = new Manager();
        
// create permissions
$perm1     = $manager->createPermission('Play to soccer');
$perm2     = $manager->createPermission('Touch the ball');

// create users
$bob       = $manager->createUser('Bob');
$john      = $manager->createUser('John');

// create roles
$adminrole = $manager->createRole('Administrator');
$userrole  = $manager->createRole('User');

```