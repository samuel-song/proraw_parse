# proraw_parse
parse iphone proraw, include main rgb data, full size preview jpeg, semantic mask

## usage
environment:<br/>
WIN10 + VS2019 toolset V142 + cmake 3.23.1
```bat
cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
cmake --build . --config=Release
```
copy your dng file to ./build/Release folder, run 
```bat
./validate.exe -v [yourdngfile]
```