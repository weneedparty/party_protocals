# GRPC Protocols

## generate the code (just example, don't run it directly!)
### flutter
```bash
cd flutter_client

protoc --dart_out=grpc:lib/src/generated --proto_path ../protocols helloworld.proto

cd ..
```

### rust
```bash
cd rust_service

cargo build --bin server

cd ..
```