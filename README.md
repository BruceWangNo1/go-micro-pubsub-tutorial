# Go Micro PubSub Tutorial

PubSub is a very important theme in Microservices. In fact, if there is no PubSub in your microservices, then what's the point of your Microservices? Right?

Benefits of PubSub Messaging from AWS:

"In modern cloud architecture, applications are decoupled into smaller, independent building blocks that are easier to develop, deploy and maintain. Publish/Subscribe (Pub/Sub) messaging provides instant event notifications for these distributed applications.

The Publish Subscribe model enables event-driven architectures and asynchronous parallel processing, while improving performance, reliability and scalability."

## Preparation

Please make sure you check out Asim's [examples](https://github.com/micro/examples) from time to time. Though it seems to be too simple, too little, it can actually help you out when you encounter a confusing bug that is keeping you from moving forward. In this case, check out **examples/broker**.

## Getting Broker

To get **broker**, do the following:

Inside srv service

```go
yourBroker := yourSRVService.Options().Broker
```

Inside web service (I spent a lot of time trying to figure out how to get broker out of web service.)

```go
yourBroker := yourWEBService.Options().Service.Options().Broker
```

Now don't forget to **Init** and **Connect** **yourBroker**:

```go
if err := b.Init(); err != nil {
	log.Fatalf("Broker Init error: %v", err)
}
if err := b.Connect(); err != nil {
	log.Fatalf("Broker Connect error: %v", err)
}
```

## Subscribing to A Topic

```go
if _, err := yourBroker.Subscribe(yourTopic, yourHandler); err != nil {
	log.Fatalf("broker.Subscribe topic %v error: %v", yourTopic, err)
}
```
## Publishing A Message under A Topic

Don't forget to import **broker** to compose messages.

```go
import "github.com/micro/go-micro/broker"
```

```go
msg := &broker.Message{
	Body: body,
}

if err := b.Publish(yourTopic, msg); err != nil {
	log.Logf("error publishing: %v", err)
}
```

## Checking whether The Message Queue You Choose Are Being Used

```go
log.Logf("broker.String(): %v", b.String())
```

If you are not using "any", **broker.String()** will be **http**. 

If you are using NATS, **broker.String()** will be **nats**.

All right. All right. All right.

