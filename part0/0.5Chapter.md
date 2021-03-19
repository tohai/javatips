# chương 6: Vấn đề về condition trong java

## 6.1 Dùng toán tử AND và OR mọt cách hợp lý

ví dụ như sau

```java
    if(products.size() == 0 || users.size() ==0  ){
        //Todo something
    }
```

Với ví dụ trên: Nếu `products.size() == 0` thì chương trình sẽ bỏ qua `users.size()` và vào thẳng luôn trong khối if.

Như vậy hay dùng tối đa điều kiện OR nếu có thể để tăng hiêu xuất code.

## 6.2 Bao nhiêu toán tử trong một câu lệnh if là hợp lý

Theo như thống kê đặt ra thì nên có tối đa là 3 phần tử trong 1 điều kiện.\
trường hợp nhiều phần từ hơn thì sao?\
Hãy phân nhóm chúng và tạo ra các hàm trả ra boolean để check chúng 1 cách hợp lý.

## 6.3 Các vấn đề nếu không quản lý chặt điều kiện

nếu không quản lý chặt các điều kiện thì ta có thể gặp phỉa 2 loại sau

1. Check điều kiện 1 cách không cần thiết

    Như ví dụ dưới đây:

    ```java
        if(products.size() == 0 || users.size() ==0  ){
            //Todo something 1
            if(products.size() == 0 || users.size() ==0  ){//(1)
            //Todo something 2
            }
        }
    ```

    bạn có thấy (1) là điều kiện thừa không. tại sao phải check nó trong khi nó là hiển nhiên.

2. dead code

    Như ví dụ dưới đây:

    ```java
        if(products.size() == 0 || users.size() ==0  ){
            //Todo something 1
            if(products.size() != 0 && users.size() !=0  ){//(1)
            //Todo something 2
            }
        }
    ```

    Rõ ràng là khố thứ 2 sẽ chẳng bao giờ chạy cả. nó gọi là dead code. tà ta hoàn toang có thể bỏ khói này đi.

với cả 2 loại ta đều thấy nó sẽ chẳng ảnh hưởng gì đến nghiệp vụ. tuy nhiên nó sẽ làm ta mất xử lý check và làm nặng size của phần mềm

## 6.4 Tổng kết

1. Hãy tối ưu điều kiện bằng cách quy ra nhiều OR
2. Số lượng phần tử trong điều kiện là hợp lý
3. Quản lý chặt các điều kiện để code được đẹp và nhanh hơn.
