# reqwest-actix-stream

A Stream to link between Reqwest and Actix-web two systems.

## PayloadStream Example

```rust
async fn handle(
    body: actix_web::web::Payload,
) {
    let mut builder = client.get(url);
    builder = builder.body(reqwest::Body::wrap_stream(reqwest-actix-stream::PayloadStream {
        payload: body,
    }));
    builder.send().await;
}
```

## ResponseStream Example

```rust
let res = builder.send().await;
let stream = res.bytes_stream();
let mut resp = HttpResponse::build(res.status());
// This method will use chunked Transfer-Encoding, otherwise use actix_web::body::SizedStream
return Ok(resp.streaming(reqwest-actix-stream::ResponseStream{ stream: stream }));
```