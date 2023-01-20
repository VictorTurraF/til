# Http Requests

## Introduction

Http requests are a common way to communicate with a server. In this tutorial we will learn how to make http requests in Java.

## Java Http Client

An example making a GET request to the [Dumy Json API](https://dummyjson.com/).

```java

URI uri = URI.create("https://dummyjson.com/products/1");

HttpRequest request = HttpRequest
    .newBuilder()
    .uri(uri)
    .GET()
    .header("accept", "application/json")
    .build();

HttpClient client = HttpClient.newHttpClient();

HttpResponse<String> response = client.send(
        request,
        HttpResponse.BodyHandlers.ofString()
);

Gson g = new Gson();
ProductDTO product = g.fromJson(response.body(), ProductDTO.class);

System.out.println("Id: " + product.id);
System.out.println("Title: " + product.title);
System.out.println("Description: " + product.description);
System.out.println("Price: " + product.price);
System.out.println("Discount Percentage: " + product.discountPercentage);
System.out.println("Rating: " + product.rating);
System.out.println("Stock: " + product.stock);
System.out.println("Brand: " + product.brand);
System.out.println("Category: " + product.category);
System.out.println("Thumbnail: " + product.thumbnail);
System.out.println("Images: " + product.images);
```

Full code in this github repository

> It's necessary to add the Gson dependency to the project to be able to parse the JSON response. In this case the response is converted to a ProductDTO object.

An example making an asynchronous GET request to the [Dumy Json API](https://dummyjson.com/).

```java
CompletableFuture<HttpResponse<String>> responseFuture = client.sendAsync(
    request,
    HttpResponse.BodyHandlers.ofString()
);
// We can do other things here while the request is in-flight
System.out.println("This line was printed while the request is in-flight");

// This blocks until the request is complete
HttpResponse<String> response3 = responseFuture.get();

ProductDTO product2 = g.fromJson(response3.body(), ProductDTO.class);

System.out.println("Last example (async):");
System.out.println("Id: " + product2.id);
System.out.println("Title: " + product2.title); // An so on...
```

## Here's the references:
- [HttpClient API documentation](https://docs.oracle.com/en/java/javase/12/docs/api/java.net.http/java/net/http/HttpClient.html).
- [HttpRequest API documentation](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/HttpRequest.html).
- [HttpResponse API documentation](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/HttpResponse.html).
- [Gson API documentation](https://github.com/google/gson).

This annotations is based in [this content](https://www.baeldung.com/java-9-http-client) and [this post of twilio blog](https://www.twilio.com/pt-br/blog/5-maneiras-de-fazer-uma-chamada-http-em-java) recomended by the [Java Developers Roadmap](https://roadmap.sh/java).