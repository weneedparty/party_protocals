# GRPC Protocols

## generate the code (just example, don't run it directly!)
### flutter
```bash
cd flutter_client

mkdir -p lib/generated_grpc

protoc --dart_out=grpc:lib/generated_grpc --proto_path ../party_protocals/protocols helloworld.proto

protoc --dart_out=grpc:lib/generated_grpc --proto_path ../party_protocals/protocols account_service.proto

cd ..
```

### rust
```rust
#build.rs

fn main() -> Result<(), Box<dyn std::error::Error>> {
    tonic_build::compile_protos("../party_protocals/protocols/helloworld.proto")?;

    tonic_build::compile_protos("../party_protocals/protocols/account_service.proto")?;

    Ok(())
}
```

```bash
cd rust_service

cargo build --bin server

cd ..
```

### python (deprecated)
```bash
export GRPC_PYTHON_BUILD_SYSTEM_OPENSSL=1
export GRPC_PYTHON_BUILD_SYSTEM_ZLIB=1
python -m pip install grpcio grpcio-tools
python -m pip install --pre "betterproto[compiler]"

mkdir src/generated_grpc
python -m grpc_tools.protoc -I ../party_protocals/protocols --python_betterproto_out=src/generated_grpc account_service.proto
```

```bash
# not recommend
export GRPC_PYTHON_BUILD_SYSTEM_OPENSSL=1
export GRPC_PYTHON_BUILD_SYSTEM_ZLIB=1
python -m pip install grpcio grpcio-tools

mkdir -p src/generated_grpc

python -m grpc_tools.protoc --proto_path ../party_protocals/protocols --python_out=src/generated_grpc --grpc_python_out=src/generated_grpc ../party_protocals/protocols/account_service.proto 
#--experimental_allow_proto3_optional

echo -e "import os \nimport sys \nsys.path.insert(0, os.path.abspath(os.path.dirname(__file__)))" >> src/generated_grpc/__init__.py
```

## port design

### old hello world code
rust: 40051

### rust gateway
rust: 40052

### account service (restful)
python: 40053