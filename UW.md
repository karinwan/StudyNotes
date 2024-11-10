### ECE Ubuntu:

```
ssh <watiam>@eceubuntu.uwaterloo.ca
```

### Gitlab - ECE650
```
git fetch upstream
```

```
git merge upstream/master --allow-unrelated-histories
```

### Clang-tidy
1. ```mkdir build``` and ```cd build``` into the build folder
2. ```cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=ON ../```
3. ```cd back``` to your a2 folder
4. ```clang-tidy -p build ece650-a4.cpp```
