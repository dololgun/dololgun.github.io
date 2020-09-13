## irb 실행

루비가 설치됐다는 가정하에 터미널에 `irb`를 입력하자.

IRB는 입력한 루비코드를 실행한 결과를 보여준다.

### puts

`println`과 비슷한 명령어로 콘솔에 특정 값을 출력한다. 리턴 값은 nil이다.

> nil은 루비에서 절대적으로 아무것도 없는 값을 의미한다.

### **

거듭 제곱을 한다. 3의 2젭곱은 `3**2`이다. 

### Math.sqrt(9)

`Math.sqrt(9)`

### =~

정규식 연산자이다. 

정규식 =~ 대상소스

```ruby
/mi/ =~ "hi mike" # => 3 
"hi mike" =~ /mi/ # => 3 

"mike" =~ /ruby/ # => nil 
```

## 매소드 정의

### def

매소드를 정의하는 지시어이다. 다음과 같이 하나의 hello라는 매소드를 정의할 수 있다.

```ruby
def hello
puts "hello world"
end
```

이렇게 입력하면 `=> :hello`가 리턴되는데 이는 루비가 매소드가 정의된 것을 이해했다는 뜻이다.

다음처럼 인자를 받는 매소드를 만들 수 있다. 

```ruby
def hello(name)
    puts "hello #{name}!"
end
```

name이란 변수를 편리하게 #{}를 이용하여 사용할 수 있다.

### class

```ruby
irb(main):024:0> class Greeter
irb(main):025:1>   def initialize(name = "World")
irb(main):026:2>     @name = name
irb(main):027:2>   end
irb(main):028:1>   def say_hi
irb(main):029:2>     puts "Hi #{@name}!"
irb(main):030:2>   end
irb(main):031:1>   def say_bye
irb(main):032:2>     puts "Bye #{@name}, come back soon."
irb(main):033:2>   end
irb(main):034:1> end
=> nil
```

`@name`은 클래스 맴버변수이다. 

객체 사용은 다음과 같다. 

```ruby
irb(main):035:0> g = Greeter.new("Pat")
=> #<Greeter:0x16cac @name="Pat">
irb(main):036:0> g.say_hi
Hi Pat!
=> nil
irb(main):037:0> g.say_bye
Bye Pat, come back soon.
=> nil
```

### 클래스 재정의하기

```ruby
irb(main):044:0> class Greeter
irb(main):045:1>   attr_accessor :name
irb(main):046:1> end
=> nil
```

attr_accessor는 2개의 매소드를 추가한다. 

1. **name** : @name 필드의 getter.
2. **name=** : @name 필드의 setter.

