# go-balancer

## 描述
go-balancer 是一个 Go 包，提供了多种负载均衡算法。

## 安装
要安装该包，请运行以下命令：
```
go get github.com/kercylan98/go-balancer
```

## 使用
以下是如何使用 go-balancer 包的示例代码：

```go
package main

import (
	"fmt"
	"github.com/kercylan98/go-balancer"
	"github.com/kercylan98/go-balancer/balancers"
)

type TestInstance struct {
	ID     balancer.InstanceID
	weight int
}

func (t *TestInstance) GetWeight() int {
	return t.weight
}

func (t *TestInstance) GetID() balancer.InstanceID {
	return t.ID
}

func NewTestInstance(id balancer.InstanceID) *TestInstance {
	return &TestInstance{
		ID: id,
	}
}

func main() {
	builder := balancers.NewRoundRobinBalancerBuilder[*TestInstance]()
	balancer := builder.Build()
	balancer.RegisterInstance(NewTestInstance("1"))
	balancer.RegisterInstance(NewTestInstance("2"))
	balancer.RegisterInstance(NewTestInstance("3"))

	for i := 0; i < 5; i++ {
		selected := balancer.Select()
		fmt.Println("Selected instance ID:", selected.GetID())
	}
}
```

## 许可证
此项目是根据 MIT 许可证授权的 - 有关详细信息，请参阅 [LICENSE](LICENSE) 文件。
