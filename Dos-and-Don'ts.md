## Accessing value in `base::ListValue`

:x: No
```cpp
void FooDOMHandler::Function(const base::ListValue* args) {
  std::string param1;
  args->GetString(0, &param1);
  int32 param2;
  args->GetInteger(0, &param2);
  service_->Function(param1, param2);
}
```

:heavy_check_mark: Yes
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