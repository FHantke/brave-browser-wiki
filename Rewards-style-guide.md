## Accessing value in `base::ListValue`

:x:
```cpp
void FooDOMHandler::Function(const base::ListValue* args) {
  std::string param1;
  args->GetString(0, &param1);
  int32 param2;
  args->GetInteger(0, &param2);
  service_->Function(param1, param2);
}
```

:heavy_check_mark:
```cpp
void FooDOMHandler::Function(const base::ListValue* args) {
  CHECK_EQ(2U, args->GetSize());
  if (rewards_service_) {
    const std::string param1 = args->GetList()[0].GetString();
    const int32 param2 = args->GetList()[1].GetInt();
    service_->Function(param1, param2);
  }
}
```

## Assigning values

:x:
```cpp
const std::string val("10");
```

:heavy_check_mark:
```cpp
const std::string val = "10";
```

## Passing/assigning empty string

:x:
```cpp
void Foo(bool ok) {
  std::string val = ":(";
  if (ok) {
    val = std::string();
  }

  FooSecond(val, std::string());
}
```

:heavy_check_mark:
```cpp
void Foo(bool ok) {
  std::string val = ":(";
  if (ok) {
    val = "";
  }

  FooSecond(val, "");
}
```

## Const for function parameters

:x:
```cpp
int Foo(int ok) {
  return ok + 1;
}
```

:heavy_check_mark:
```cpp
int Foo(const int ok) {
  return ok + 1;
}
```


## Brackets for single lines

:x:
```cpp
if (true)
  return;
```

:heavy_check_mark:
```cpp
if (true) {
  return;
}
```
