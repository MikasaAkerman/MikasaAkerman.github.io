### a != nil

- 一个指针为nil需要满足两个条件
  1. 类型为空
  2. 值为空

- 以下代码会Fail

```go
func TestError(t *testing.T) {
	err := ret()
	if err != nil {
		t.Fatal(err)
	}
}

func ret() error {
	var sts *Status
	return sts
}

type Status struct {
	Msg string
}

// Error implement error
func (s *Status) Error() string {
	return s.Msg
}
```

- 判断值为空的方法

  ```go
  func IsNil(i interface{}) bool {
      vi := reflect.ValueOf(i)
      if vi.Kind() == reflect.Ptr {
          return vi.IsNil()
      }
      return false
  }
  ```

- 所以这样写代码是错误的

  ```go
  func ret() (int, error) {
  	return method()
  }
  
  func method() (int, *Status) {
  	return 0, nil
  }
  
  type Status struct {
  	Msg string
  }
  
  // Error implement error
  func (s *Status) Error() string {
  	return s.Msg
  }
  ```

- 正确的代码应该是这样的

  ```go
  func ret() (int, error) {
  	i, sts := method()
  	if sts != nil {
  		return 0, sts
  	}
  	return i, nil
  }
  
  func method() (int, *Status) {
  	return 0, nil
  }
  
  type Status struct {
  	Msg string
  }
  
  // Error implement error
  func (s *Status) Error() string {
  	return s.Msg
  }
  ```

  