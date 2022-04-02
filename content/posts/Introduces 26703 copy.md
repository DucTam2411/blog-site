---
title: "Second"
date: 2022-04-03T01:35:07+07:00
draft: false
---

# Introduces to Kubernetes

# I. The need of Kubernetes

## 1. Moving from monolithic app to microservice

Monolithic applications consist of components that are all tightly coupled together and have to be developed, deployed,  and managed as one entity, because they all run as a single OS process.

- If changes → redeployment
- Requires small number of **powerful servers**
- Increasing loads → vertical scale → add more CPUs, memory...

### Splitting Apps into Microservices

![Untitled](/Introduces%2026703/Untitled%202.png)

These and other problems have forced us to start splitting complex monolithic applications into smaller **independently deployable components** called microservices. 

Each microservice runs as an **independent** process (see figure 1.1) and communicates with other microservices through simple, well-defined interfaces, it’s possible to develop and deploy each microservice separately.

### Scaling Microservices

Scaling microservices, unlike monolithic systems, where you need to scale the system as a whole, is done on a per-service basis, which means you have the option of **scaling only those services that require more resources**, while leaving others at their original scale.

![Untitled](/Introduces%2026703/Untitled%201.png)

Certain components are replicated and run as multiple processes deployed on different servers, while others run as a single application process. When a monolithic application can’t be scaled out because one of its parts is unscalable, splitting the app into microservices allows you to horizontally scale the parts that allow scaling out, and scale the parts that don’t, vertically instead of horizontally.

### Deploying Microservices

Microservices also have drawbacks

- many separate components →  deployment-related decisions become increasingly difficult
- microservices perform work as a team( deploy new component → configure old components)
- hard to debug and trace execution calls( cause they span many process and machines)

### Understand the divergence of environment requirements

  

![Untitled](/Introduces%2026703/Untitled%202.png)

Deploying  dynamically  linked  applications  that  require  different  versions  of  shared libraries,  and/or  require  other  environment  specifics,  can  quickly  become  a  nightmare  for  the  ops  team  who  deploys  and  manages  them  on  production  servers.  The bigger the number of components you need to deploy on the same host, the harder it will be to manage all their dependencies to satisfy all their requirements.

## 2. Providing a consistent environment to applications

One of the biggest problems that dev and operation teams always have to deal with is the **differences in the environment they run their apps in**.

- different between production and development environments
- different between individual production machines
- environment machine will change over time.

These differences range from hardware to software( like OS) that are available on each machine. Production environments  are  managed  by  the operations team, while developers often take care of their  development laptops on their own.

# II. Introducing to container technologies

![Untitled](/Introduces%2026703/Untitled%203.png)

## Understanding what containers are

### Isolation components with Linux Container technologies

Instead of using virtual machines to isolate the environments of each microservice (or software  processes  in  general),  developers  are  turning  to  Linux  container  technologies. They allow you to run multiple services on the same host machine, while not only exposing a different environment to each of them, but also isolating them from each other, similarly to VMs, but with much less overhead.

### Comparing VMs to Containers

![Untitled](/Introduces%2026703/Untitled%204.png)

When you run three VMs on a host, you have three completely separate operating systems running on and sharing the same bare-metal hardware. Underneath those VMs is the host’s OS and a hypervisor, which divides the physical hardware resources into smaller sets of virtual resources that can be used by the operating system inside each VM.

| VMs                                                                                                                                                                                                                                                                                                                                                | Container                                                                                                                                                                               |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| When you run three VMs on a host, you have three completely separate operating systems running on and sharing the same bare-metal hardware. Underneath those VMs is the host’s OS and a hypervisor, which divides the physical hardware resources into smaller sets of virtual resources that can be used by the operating system inside each VMs. | Containers, on the other hand, all perform system calls on the exact same kernel running in the host OS. The CPU doesn’t need to do any kind of virtualization the way it does with VMs |
| Full isolation - because each VM runs its own Linux kernel.                                                                                                                                                                                                                                                                                        | Containers all call out to the same kernel, which can clearly pose a security risk.                                                                                                     |
| If you have a limited amount of hardware resources, VMs may only be an option when you have a small number of processes that you want to isolate.                                                                                                                                                                                                  | To run greater numbers of isolated processes on the same machine, containers are a much better choice because of their low overhead.                                                    |

### What make Containers possible

Two mechanisms make this possible

- Linux Namespaces
    
    Makes sure each process sees its own personal view of the  system  (files,  processes,  network  interfaces,  hostname,  and  so  on).
    
- Linux Control Groups (cgroups)
    
    Limit the amount of resources the process can consume (CPU, memory, network bandwidth, and so on).
    

# III. Giới thiệu về Kubernetes

Chúng ta thấy được sự khó khăn trong việc quản lý các component( deployable application components) khi hệ thống dần được mở rộng. Google đã nhận thấy các công ty cần một cách tốt hơn để quản lý và depoy các components tốt hơn.

## 1. Nguồn gốc

Qua nhiều năm, Google đã phát triển một hệ thống gọi là **Borg** (sau này là Omega) giúp cho các nhà phát triển (dev) và admin hệ thống quản lý các hàng ngàn ứng dụng và service. Ngoài việc đơn giản hóa quá trình deploy và phát triển, nó còn giúp tối ưu hạ tầng cơ sở(infrastructure).  

Năm 2014, Google đã giới thiệu Kubernetes dựa trên kinh nghiệm phát triển Borg, Omega.

## 2. Tổng quan về Kubernetes

Kubernetes là một hệ thống phần mềm giúp chúng ta dễ dàng deploy và quản lý các **ứng dụng chạy trong container**(containerized application)

Nó có thể chạy các ứng dụng trong container mà không cần phải biết bất kì thông tin gì về chúng, đồng thời ta không phải deploy thủ công những ứng dụng này vào các host. Bởi vì các ứng dụng này được chạy trong các container khác nhau, chúng độc lập nhau → ta có thể chạy nhiều ứng dụng khác nhau trên 1 phần cứng.

Kubernetes cho phép bạn chạy ứng dụng trên hàng ngàn máy tính như đó chỉ là một máy tính duy nhất. Nó abstract đi hệ thống cơ sở hạ tầng bên dưới(infrastructure) → đơn giản hóa quá trình phát triển, deploy và maintain.

Deploy một ứng dụng trên Kubernetes luôn giống nhau cho dù cluster đó có 1-2 node hoặc 1000 node.

### Cốt lõi của Kubernetes

 

![Untitled](/Introduces%2026703/Untitled%205.png)

Đây là góc nhìn đơn giản nhất về Kubernetes. Hệ thống bao gồm 1 **master node** và nhiều **work node**. Khi dev deploy hệ thống, Kubernete sẽ deploy chúng vào 1 cluster gồm các worker node. Node nào sẽ chạy components thì chúng ta (và cả admin hệ thống) không cần phải quan tâm.

### Giúp Developer tập trung vào các chức năng chính (core app features)

Kubernetes giống như một hệ điều hành (OS) cho **cluster**. Nó giải phóng developer khỏi việc tự cài đặt các **services về cơ sở hạ tầng**, mà chỉ tập trung vào việc phát triển các chức năng của phần mềm. Thay vào đó, họ sẽ sử dụng các service có sẵn của Kubernetes - bao gồm scaling, load-balancing, self - healing,...

### Giúp đội ngũ vận hàng (Ops team) tối ưu tài nguyên

## 3. Kiến trúc của Kubernetes cluster

Ở mức độ phần cứng, Kubernetes cluster được cấu thành từ nhiều **node**, chúng ta có thể phân loại thành 2 loại node:

1. master node, có **Kubernetes Control Plane** để quán lý cả hệ thống Kubernetes
2. worker node, để chạy ứng mà chúng ta deploy

![Các thành phần tạo nên 1 Kubernetes cluster](/Introduces%2026703/Untitled%206.png)

Các thành phần tạo nên 1 Kubernetes cluster

### The control plane - master node

Giữ nhiệm vụ quản lý cluster mà giữ cho cluster hoạt động. Nó bao gồm các component:

- **Kubernetes API Server**: giúp ta giao tiếp với các component của control plane khác
- **Scheduler**: lập lịch cho ứng dụng (giao component cho cho một worker node)
- **Controller Manager**: Xử lí các chức năng ở mức cluster (cluster-level function) như quản lý các worker node, tái tạo các component, xử lý các node lỗi,...
- **etdc**: dữ liệu về các cài đặt cluster và trạng thái (state) của cluster

The control plane nắm giữ và quản lý các trạng thái của cluster, nhưng chúng không chạy các ứng dụng. Điều đó được thực hiện bởi worker node.

 

### Worker node

Worker node là cỗ máy (machine) chạy các ứng dụng trong container. Các tác vụ vận hành, quản lý và cung cấp các dịch vụ cho ứng dụng được thực thi bởi các component sau:

- Docker, rkt hoặc là bất kì container runtime khác
- kubelet, giao tiếp với cluster và quản lý các container trên node
- Kubernetes Service Proxy (kube-proxy), cân bằng tải giữa các components

## 4. Chạy một ứng dụng trên Kubernetes

Muốn chạy một ứng dụng trên Kubernetes, ta phải đóng gói nó lại thành một image container, push image đó lên image registry, và đăng miêu tả (description) về image đó lên **Kubernetes API server**.

 

### Mô tả trong một container đang hoạt động

![Untitled](/Introduces%2026703/Untitled%207.png)

Khi API server xử lí miêu tả về ứng dụng của bạn, Scheduler sẽ lập lịch tạo ra các container trên các work node dựa trên nguồn tài nguyên hiện có của bạn. **Kubelet** trên các node sẽ giao tiếp với với Kubernetes API Server để tải các image và chạy các image đó.

### Giữ cho các container luôn vận hành

Khi ứng dụng được đưa vào hoạt động (deployed), Kubernetes sẽ liên tục đảm bảo rằng trạng thái của ứng dụng đúng với miêu tả mà bạn cung cấp. Ví dụ như bạn luôn muốn có 5 instance của web server hoạt động → Kubernetes sẽ luôn đảm bảo 5 instance. Nếu 1 instance ngừng hoạt động hay không phản hồi → Kubernetes sẽ tự restart nó

Tương tự, nếu 1 node chết đi → Kubernetes sẽ chọn 1 node khác và chạy lại tất cả container của nó.

### Scaling các bản sao chép (copies)

Khi ứng dụng đang chạy, bạn có thể quyết định tăng giảm số lượng copies tùy thích → Kubernetes sẽ cập nhật theo yêu cầu. Thậm chí, bạn có thể để Kubernetes điều này, dựa vào các thông số thực tế (real-times metrics) như CPU load, tiêu thụ bộ nhớ, lượng truy vấn từng giây,...

## 5. Lợi ích của việc sử dụng Kubernetes

Nếu bạn deploy Kubernetes trên tất cả server, đội ngũ vận hành (Ops team) sẽ không cần xử lí việc deploy các ứng dụng nữa. Bởi vì các ứng dụng chạy trong container (containerized application) đã được bổ sung các tất cả những gì nó cần để chạy → admin hệ thống không cần phải cài đặt thứ gì để deploy và chạy ứng dụng. 

### Đơn giản hóa quá trình deploy

Bởi vì Kubernetes cung cấp tất cả worker node như là một **deployment platform**, developer có thể bắt đầu deploy ứng dụng mà không cần biết gì về các server tạo nên cluster.

 Một vài trường hợp đặc biệt, như khi ta muốn ứng dụng của mình được chạy trên một node sử dụng SDD thay vì HDD.

- Nếu không sử dụng k8s → admin hệ thống sẽ phải chọn 1 node cụ thể và deploy ứng dụng ở đó
- Khi sử dụng k8s → ta có thể nói cho k8s chọn 1 node sử dụng SDD và k8s sẽ tự động vận hành theo yêu cầu.

### Tối ưu phần cứng

Bằng việc set up k8s trên server, chúng ta sẽ decouple ứng dụng khỏi kiến trúc. Khi ta yêu cầu k8s chạy ứng dụng, k8s sẽ chọn node phù hợp nhất dựa trên **mô tả tài nguyên yêu cầu** (description  of  the  application’s resource requirements) và **nguồn tài nguyên hiện có trên các node**.

Bằng việc sử dụng các container và không cố định các node trên cluster, ta cho phép ứng dụng tự do vận chuyển trên cluster → các component khác nhau sẽ được sắp xếp sao cho tối ưu phần cứng nhất trên các node.

### Kiểm tra và tự hồi phục

Ứng dụng tự do vận chuyển trên cluster là một yếu tố đáng giá trong trường hợp các server của chúng ta bị hỏng (server failure). Nếu quy mô của cluster chúng ta tăng lên → các component sẽ vận hành thất bại thường xuyên hơn.

Kubernetes sẽ quản lý các node và các components đang chạy trên đó, và sẽ tự động lập lịch các components đó trên các node khác nếu server bị lỗi. Điều này cho phép đội vận hành tập trung vào việc xử lí vấn đề node lỗi thay vì phải tìm cách tích hợp các components cũ vào ứng dụng.

### Automatic scaling

Sử dụng Kubernetes để quản lý các ứng dụng đã deploy cho phép đội vận hành không cần phải liên tục quản lý lượng truy cập ứng dụng, Kubernetes sẽ quản lý nguồn tài nguyên sử dụng và liên tục cập nhật  số lượng instance của mỗi ứng dụng

Nếu Kubernetes chạy trên cơ sở hạng tầng đám mây (cloud infrastructrue), việc thêm các node sẽ vô cùng đơn giản. Thậm chí, Kubernetes có thể tự động scale kích thước của cluster tùy vào nhu cầu của ứng dụng.

### Đơn giản quá trình phát triển

Ứng dụng sẽ được phát triển và vận hành trong cùng một môi trường, điều này có ý nghĩa rất lớn trong việc phát hiện bugs. Đồng thời, developer không cần implemement các service mà Kubernetes đã cung cấp sẵn như cân bằng tải, tìm kiếm các service,...

Đồng thời, Kubernetes cũng cung cấp thêm khả năng rollout lại ứng dụng, k8s có thể tự phát hiện nếu 1 version bị lỗi → tự rollout lập tức. Điều này tăng một tầng bảo đảm cho các developer.

# IV. Tổng kết

- Monolithic app dễ deploy, nhưng khó maintain mà đôi khi là không thể scale
- Microservices kiến trúc cho phép phát triển mỗi component dễ hơn → khó deploy và configure để chúng tương tác với nhau như một hệ thống đơn nhất.
- Linux containers có đem lại lợi ích như VMs nhưng nhẹ hơn và cho phép tối ưu phần cứng tốt hơn.
- Kubernetes phơi bày (expose) cả datacenter như một single  computational  resource for running applications.
- Dev có thể deploy apps bằng k8s một cách đơn giản
- Admin hệ thống sẽ được sẽ được ngủ ngon hơn khi Kubernetes tự động xử lí khi server gặp sự cố.