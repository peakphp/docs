### How method create() work
The method create() will help you to instantiate objects. It is important to understand that enabling/disabling autowiring affect how this method create objects.
Autowiring is enabled by default. To change this, use ```disableAutowiring()```.

Under the hood, ```create()``` go through those steps in order:

When Autowiring is enabled :
- Check constructor type-hinted argument(s) using Reflection
- Check for $explicit definition(s) to overload/guide the resolver
- If no $explicit, look for a stored instance in the container, or instantiate a new one
   
When Autowiring is disabled:
- Check for $explicit definition(s) to overload/guide the resolver.
- If no $explicit, look for a matching definition and resolve it.


#### Parameters
```PHP
create(string $class [, array $args = [] [, mixed $explicit = null ]]])
```

#### ```$class``` 

Represent the class string name you want to create.

#### ```$args```

Represent other(s) non-object parameters if apply (or arguments if you prefer).

```PHP
class Foo {
    public function __construct(Bar $bar, $id = null, $desc = null) {
        $this->bar = $bar;
    }
}

$foo = $container->create(Foo::class, [
    12, // $id
    'FooBar' // $desc
]);
```

#### ```$explicit```

Because autowiring is not always able to resolve an interface, you need to specify in those cases how the container should resolve it.

Also, because this parameters can be also used to bypass a definition and/or a stored instance, you should only use it when you have no other choice. A better choice would be to rethink which object need to be stored or disable autowiring and use bind definition to control more precisely your objects creations.

```PHP
interface InterfaceA {}
class A implements InterfaceA {}
class B implements InterfaceA {}

class Foo {
    public function __construct(InterfaceA $a) {
        $this->a = $a;
    }
}

// throw an exception, there is no InterfaceA stored in container
$foo = $container->create(Foo::class);

// by adding class A instance, the container is now able to resolve Foo correctly
$container->add(new A);
$foo = $container->create(Foo::class);

// now we add another class that implement InterfaceA so you
// need to specify which one to use, otherwise, it will throw an exception
$container->add(new B);
$foo = $container->create(Foo::class, [], [
    InterfaceA::class => A::class
]);

```