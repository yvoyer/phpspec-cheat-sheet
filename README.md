PHPSpec Cheat Sheet
===================
A cheat sheet for [phpspec](https://github.com/phpspec/phpspec) (>= v2.4.0)

# Console
* Generate a new specification: `bin/phpspec desc Full/Class/Name`
* Run the tests: `bin/phpspec run`

# Matchers

## Same (===)

``` PHP
$this->getMethodName()->shouldReturn('value');
$this->getMethodName()->shouldBe('value');
$this->getMethodName()->shouldEqual('value');
$this->getMethodName()->shouldBeEqualTo('value');
```

## Equals (==)

``` PHP
$this->getMethodName()->shouldBeLike('value');
```

## Contain (array, string)

``` PHP
$this->getArrayValue()->shouldContain('Jane Smith'); // Element in array (===)
$this->getArrayValue()->shouldHaveKeyWithValue('key', 'value');
$this->getArrayValue()->shouldHaveKey('key');

$this->getStringValue()->shouldContain('value'); // String contain exact value (===)
$this->getStringValue()->shouldStartWith('value');
$this->getStringValue()->shouldEndWith('value');
$this->getStringValue()->shouldMatch('/pattern/'); // Regex
```

## Object type (instanceof)

``` PHP
$this->getMethodName()->shouldHaveType('\Full\Class\Name');
$this->getMethodName()->shouldReturnAnInstanceOf('\Full\Class\Name');
$this->getMethodName()->shouldBeAnInstanceOf('\Full\Class\Name');
$this->getMethodName()->shouldImplement('\Full\Interface\Name');
```

## Bool

For methods named `is*` or `has*`.

``` PHP
$this->shouldBeActive(); // isActive() method should return true
$this->shouldHaveSomething(); // hasSomething() method should return true

$this->shouldNotBeActive(); // isActive() method should return false
$this->shouldNotHaveSomething(); // hasSomething() method should return false
```

## Countable

``` PHP
$this->getCountableObject()->shouldHaveCount(1);
$this->getArrayValue()->shouldHaveCount(1);
```

## Exception

``` PHP
$this->shouldThrow('\Exception')->duringMethodName('value');
$this->shouldThrow(new \Exception('Message'))->duringMethodName('value');

$this->shouldThrow('\Exception')->during('methodName', array('value'));
$this->shouldThrow(new \Exception('Message'))->during('methodName', array('value'));

$this->shouldThrow('\Exception')->duringInstantiation();
$this->shouldThrow(new \Exception('Message'))->duringInstantiation();
```

## Scalar

``` PHP
$this->getBool()->shouldBeBool();
$this->getObject()->shouldBeObject();
$this->getString()->shouldBeString();
$this->getInteger()->shouldBeInteger();
$this->getDecimal()->shouldBeDecimal();
$this->getCollection()->shouldBeArray();
```

## Object construct

``` php
$this->beConstructedWith(-3);
$this->beConstructedThrough('methodName', array(-3));
```

## Custom matcher
Method should return a value that match the custom matcher.

``` PHP
function it_should_have_some_specific_options_by_default()
{
    $this->getOptions()->shouldHaveKey('username');
    $this->getOptions()->shouldHaveValue('diegoholiveira');
}

public function getMatchers()
{
    return [
        'haveKey' => function($subject, $key) {
            return array_key_exists($key, $subject);
        },
        'haveValue' => function($subject, $value) {
            return in_array($value, $subject);
        },
    ];
}
```

# Stubs
Collaborator (Composed object) should call a method that could return a value.

``` PHP
/**
 * @param Full\Class\Name $stub
 */
function it_adds_a_end_of_list_to_markup($stub)
{
    $stub->getExpectedMethod()->willReturn("Some value");
    $this->testMethod("value", $stub)->shouldReturn("other value");
}
```

# Mocks
Expecting a method to be called on an object.

``` PHP
/**
 * @param SomeMock $expectedMock
 * @param SomeStub $expectedStub
 */
function it_should_call_a_method_on_subscriber($expectedMock, $expectedStub)
{
    $expectedMock->methodName($expectedStub)->shouldBeCalled();

    // when
    $this->addStub($expectedStub);
    $this->doMethod($expectedMock);
}
```

# let/letgo
Construct expectations before and after each tests.

``` PHP
    function let($object)
    {
        $object->beADoubleOf('Full\Class\Name');
        $this->beConstructedWith($object);
    }

    function it_live_and_let_die($object)
    {
        $this->getSomeObject()->shouldReturn($object);
    }

    function letgo()
    {
        // release any resource
        // put the system back into the state it was before the example
    }
```

