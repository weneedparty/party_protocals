# GRPC Protocols

## generate the code (just example, don't run it directly!)
### flutter
```bash
cd flutter_client

mkdir -p lib/generated_grpc
protoc --dart_out=grpc:lib/generated_grpc --proto_path ../party_protocals/protocols helloworld.proto

cd ..
```

### rust
```rust
#build.rs

fn main() -> Result<(), Box<dyn std::error::Error>> {
    tonic_build::compile_protos("../party_protocals/protocols/helloworld.proto")?;
    Ok(())
}
```

```bash
cd rust_service

cargo build --bin server

cd ..
```