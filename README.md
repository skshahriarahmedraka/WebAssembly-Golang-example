## compile GO file and copy paste wasm_exec.js in same directory
    GOARCH=wasm GOOS=js go build -o main.wasm main.go
    cp "$(go env GOROOT)/misc/wasm/wasm_exec.js" .

## execute in NodeJS 
    GOOS=js GOARCH=wasm go build  -o main.wasm main.go 
    cp "$(go env GOROOT)/misc/wasm/wasm_exec.js" .
    node wasm_exec.js main.wasm 

## reduce wasm file size 
    ### install tinygo
    brew tap tinygo-org/tools
    brew install tinygo

    tinygo build -target wasm  -o main.wasm
    tinygo build   -o main2.wasm -target wasm ./main.go
    tinygo build   -o main2.wasm -target wasm ./main.go --no-debug
    cp "$(tinygo env TINYGOROOT)/targets/wasm_exec.js" . 

## simple Golang  server 
    go get -u github.com/shurcooL/goexec
    go get -u github.com/shurcooL/go-goon
    goexec 'http.ListenAndServe(`:8080`,http.FileServer(http.Dir(`.`)))'
