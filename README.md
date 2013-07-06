PHPSpec Cheat Sheet
===================
A cheat sheet for phpspec
# Console
* Generate a new specification: `bin/phpspec desc Full/Class/Name`
* Run the tests: `bin/phpspec run`
# Matchers
## Identity `===`

Method should return the same `===` value to expected value.

    $this
        ->getMethodName()
        ->shouldReturn('value');

    $this
        ->getMethodName()
        ->shouldBe('value');

    $this
        ->getMethodName()
        ->shouldEqual('value');

    $this
        ->getMethodName()
        ->shouldBeEqualTo('value');
## Comparaison
Method should return a value equal `==` to expected value.
    $this
        ->getMethodName()
        ->shouldBeLike('value');
## Throw
Method should throw the expected Exception.

    $this
        ->shouldThrow('\Exception')
        ->duringMethodName('value');

    $this
        ->shouldThrow('\Exception')
        ->during('methodName', array('value'));

    $this
        ->shouldThrow(new \Exception('Message'))
        ->during('methodName', array('value'));
## Type
Object is an object of type `\Full\Class\Name`.

    $this->shouldHaveType('\Full\Class\Name');
    $this->shouldReturnAnInstanceOf('\Full\Class\Name');
    $this->shouldBeAnInstance('\Full\Class\Name');
    $this->shouldImplement('\Full\Interface\Name');
## Object State
Method named `is*` or `has*` should return true.

    // isActive() method should return true
    $this->shouldBeActive();
    // hasSomething() method should return true
    $this->shouldHaveSomething();

    // isActive() method should return false
    $this->shouldNotBeActive();
    // hasSomething() method should return false
    $this->shouldNotHaveSomething();
## Count
Method should return an object of type `\Countable` or `array`, that contains x element.

    $this->getCollection()->shouldHaveCount(1);
## Scalar
Method should return a value of primitive types.

    $this->getBool()->shouldBeBool();
    $this->getObject()->shouldBeObject();
    $this->getString()->shouldBeString();
    $this->getInteger()->shouldBeInteger();
    $this->getDecimal()->shouldBeDecimal();
    $this->getCollection()->shouldBeArray();
## Custom matcher
Method should return a value that match the custom matcher.

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
# Stubs
Collaborator (Composed object) should call a method that could return a value.
    /**
     * @param Full\Class\Name $stub
     */
    function it_adds_a_end_of_list_to_markup($stub)
    {
        $stub->getExpectedMethod()->willReturn("Some value");

        $this->testMethod("value", $stub)->shouldReturn("other value");
    }
# Mocks
Expecting a method to be called on an object.

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
# let/letgo
Construct expectations before and after each tests.

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
