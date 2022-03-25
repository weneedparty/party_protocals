# GRPC Protocols

## generate the code (just example, don't run it directly!)
### flutter
```bash
cd flutter_client

mkdir -p lib/generated_grpc

protoc --dart_out=grpc:lib/generated_grpc --proto_path ../party_protocals/protocols helloworld.proto

protoc --dart_out=grpc:lib/generated_grpc --proto_path ../party_protocals/protocols account_service.proto


protoc --dart_out=grpc:lib/generated_grpc --proto_path ../party_protocals/protocols room_control_service.proto
cd ..
```

### rust
```rust
#build.rs

fn main() -> Result<(), Box<dyn std::error::Error>> {
    tonic_build::compile_protos("../party_protocals/protocols/helloworld.proto")?;

    tonic_build::compile_protos("../party_protocals/protocols/account_service.proto")?;

    tonic_build::compile_protos("../party_protocals/protocols/room_control_service.proto")?;

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

### typescript for nodejs
```bash
yarn add ts-protoc-gen@next -D

yarn add grpc-tools --ignore-scripts

pushd node_modules/grpc-tools
node_modules/.bin/node-pre-gyp install --target_arch=x64
popd


mkdir -p src/generated_grpc

PROTOC_GEN_TS_PATH="./node_modules/.bin/protoc-gen-ts"
PROTOC_GEN_GRPC_PATH="./node_modules/.bin/grpc_tools_node_protoc_plugin"
OUT_DIR="./src/generated_grpc"

protoc \
    --proto_path ../party_protocals/protocols \
    --plugin="protoc-gen-ts=${PROTOC_GEN_TS_PATH}" \
    --plugin=protoc-gen-grpc=${PROTOC_GEN_GRPC_PATH} \
    --js_out="import_style=commonjs,binary:${OUT_DIR}" \
    --ts_out="service=grpc-node,mode=grpc-js:${OUT_DIR}" \
    --grpc_out="grpc_js:${OUT_DIR}" \
    room_control_service.proto
```

## port design

### old hello world code (grpc)
    rust: 40051

### account service (restful)
    python: 40052

### room controller (grpc)
    typescript: 40053

    livekit_control_service: 7880

    livekit_user_direct_connect_port: 
        * 7881 tcp
        * 7882 udp

### gateway (grpc)
    account service: 40054
    room controller: 40055

## run
```
- configs
    - user_database
    config.py
    livekit.yaml
    o365_token.txt
```


```bash
docker-compose up

#docker-compose up -d
```