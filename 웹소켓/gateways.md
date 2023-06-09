# Gateways

의존성주입, 데코레이터, 예외처리필터, 파이프, 가드 그리고 인터셉터는 해당 문서의 다른 부분에서 다룬 대부분의 개념은 게이트웨이에 동일하게 적용됩니다.

만약 가능한 경우에는 Nestjs는 구현체의 세부 정보를 추상화해서 동일한 구성요소가 HTTP, 웹소켓, 마이크로서비스에서 실행될수 있게 도와줍니다.

해당 섹션에는 웹소켓과 관련된 Nestjs의 측면에 대해서 설명하게 됩니다.

Nestjs에서 게이트웨이는 @WebSocketGateway() 데코레이터로 선언되는 간단한 클래스 입니다.

기술적인면에 있어서 게이트웨이는 플랫폼에 영향을 받지 않으므로 어댑터가 생성될 경우 모든 웹소켓 라이브러리와 호환됩니다.

Nestjs에서는 바로 사용이 가능한 웹소켓 플랫폼으로는 socket.io와 ws가 있습니다.

개발자가 프로젝트에 필요한 웹소켓 플랫폼을 선택해서 사용할 수 있습니다.

또한 해당 가이드를 통해 직접 어댑터를 구성할수도 있습니다.

<img src="https://docs.nestjs.com/assets/Gateways_1.png">
<br/>
<br/>

> 게이트웨이는 provider로 취급될수도 있습니다. 즉 이말은 클래스 생성자를 통해서 의존성 주입이 가능합니다. 또한 게이트웨이는 다른 클래스(providers, controller)에도 주입이 가능합니다,

# 설치

우선 웹소켓 기반의 어플리케이션 작성을 위해서는 먼저 패키지를 설치해야 합니다.

```
npm i --save @nestjs/websockets @nestjs/platform-socket.io

```

# 개요

우선 앱이 웹 프로그램이 아니거나 포트를 수동으로 변경하는 경우를 제외하곤 게이트웨이는 기본적으로 HTTP 서버와 동이란 포트에서 수신하게 됩니다.

또한 데코레이터에 파라미터로 원하는 포트를 넣어주게 되면 기본 동작을 수정할 수 있습니다.

또한 게이트웨이에 namespace(소켓의 통신채널) 설정도 가능합니다.

```typescript
// 선언 방식
@WebSocketGateway(80)
@WebSocketGateway(80, {namespace: 'events'})

// WebSocketGateway 구현부
export declare function WebSocketGateway(port?: number): ClassDecorator;
export declare function WebSocketGateway<T extends Record<string, any> = GatewayMetadata>(options?: T): ClassDecorator;
export declare function WebSocketGateway<T extends Record<string, any> = GatewayMetadata>(port?: number, options?: T): ClassDecorator;
```

> 게이트웨이의 경우 모듈 내부에 providers 배열에서 참조되기 전까지는 인스턴스화가 이루어지지 않습니다.

<br/>

@WebSocketGateway() 데코레이터의 두번째 파라미터로 소켓에서 지원하는 옵션을 전달할 수 있습니다.

```typescript
@WebSocketGateway(80, {namespace: 'events'})
```
