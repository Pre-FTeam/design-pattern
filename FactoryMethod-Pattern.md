## 팩토리 메서드 패턴이란?
    💡 부모 클래스에 알려지지 않은 구체 클래스를 생성하는 패턴이며, 자식 클래스가 어떤 객체를 생성할지를 결정하도록 하는 패턴

특정 상황에서 조건에 따라서 부모 클래스의 구현체를 다르게 하여 생성해야할 때가 있습니다. 클라이언트에서 조건에 따라 분기하여 생성하면 되겠지만, 클라이언트가 많아진다면 어떨까요? 분기하여 생성하는 로직의 코드가
클라이언트마다 반복되고, 추가적인 분기점이 생긴다면 모든 클라이언트 코드를 수정해야하는 일이 생깁니다. 예시를 한번 들어 보겠습니다.

저희가 이번에 했던 계산기로 예시를 들어보겠습니다.

![Untitled](https://user-images.githubusercontent.com/82152173/198323614-2e4283b8-57e1-44a2-bdf5-87aa91edaee4.png)

최상위 클래스인 TokenType와 계산식을 잘랐을 때 계산식이 가질 수 있는 타입의 종류를 의미합니다. TokenType의 구현체에 따라서 하는 일이 다른데, 그래서 클라이언트에서 TokenType를 생성한다면
그때마다 분기를 통해 아래와 같이 생성해줘야할 것입니다.

```java
public class Client {
	public TokenType createToken(String token) {
		TokenType tokenType;

		if (isNumber(token)) {
			tokenType = new Numbers(token);
		} else if (isPlus(token)) {
			tokenType = new PlusOperator(token);
		} else if (isMinus(token)) {
			tokenType = new MinusOperator(token);
		} else if (isMultiply(token)) {
			tokenType = new MultiplyOperator(token);
		} else if (isDivide(token)) {
			tokenType = new DivideOperator(token);
		} else if (isOpenBracket(token)) {
			tokenType = new OpenBracket(token);
		} else {
			tokenType = new CloseBracket(token);
		}

		return tokenType;
	}
}
```

이렇게 사용해야하는 클라이언트가 하나라면 상관 없겠지만, 클라이언트가 많아진다면 어떨까요? 클라이언트에서 항상 생성하여 사용하게 되면 위의 코드가 계속 반복될 것입니다. 또, 계산기의 기능이 확장되어 연산자를
추가해야한다고 하면, 클라이언트마다 전부 수정해줘야합니다.

그렇다면 이제 팩토리 메서드 패턴을 적용해봅시다.

```java
public class TokenTypeFactory {
	public TokenType createType(String token) {
		TokenType tokenType;

		if (isNumber(token)) {
			tokenType = new Numbers(token);
		} else if (isPlus(token)) {
			tokenType = new PlusOperator(token);
		} else if (isMinus(token)) {
			tokenType = new MinusOperator(token);
		} else if (isMultiply(token)) {
			tokenType = new MultiplyOperator(token);
		} else if (isDivide(token)) {
			tokenType = new DivideOperator(token);
		} else if (isOpenBracket(token)) {
			tokenType = new OpenBracket(token);
		} else {
			tokenType = new CloseBracket(token);
		}

		return tokenType;
	}
}
```

이렇게 하나의 팩토리를 만들어서 몰아넣을 수도 있고, 이 중에서도 연산자(+, -, /, x)는 추가로 확장될 수 있으니 연산자 전용 팩토리를 분리하여 관리할 수도 있고, 혹은 TokenTypeFactory를
인터페이스로 하고 하위 모든 타입에 대한 팩토리를 만들어 구현하도록 만들 수도 있습니다.

```java
public class Client {
	public TokenType createType(String token) {
		TokenTypeFactory factory = new TokenTypeFactory();
		TokenType tokenType = factory.createType(token);

		return tokenType;
	}
}
```

패턴을 적용하기 전 클라이언트가 하던 일을 팩토리에 위임하여 대신하게 합니다.

## 장점

- 객체를 생성하는 일을 분리시켜 객체에 대한 수정이 일어나도 객체를 생성하는 팩토리 부분만 수정하면 됩니다.
- 어떤 경우에 어떤 구현체가 생성되는지 클라이언트 측에서 모르므로 객체 생성에 관한 캡슐화가 이루어졌습니다.
