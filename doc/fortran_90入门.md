# Fortran 90入门

## 1. 变量的定义

```fortran
integer :: a, b
real :: rr = 3.7539
real :: l_list(4) = [3.5, 4.7, 6.5, 2.5]	!l_list(4)的首索引是从1开始的
real*16 :: flo_num = 23.44455546547
dimension(:)
allocatable :: al_list ! allocate(al_list(b)) 可以先声明，后确定内存中数组的长度
```

## 2. 模块的定义

```fortran
! main.f90
program main
	use constant 
	use calculate
	real :: res
	call add(3.0, 4.0, res)
	
end program main

! constant.f90
module constant
	integer :: a, b
	real :: rr = 3.7539
	real :: l_list(4) = [3.5, 4.7, 6.5, 2.5]	!l_list(4)的首索引是从1开始的
	real*16 :: flo_num = 23.44455546547
	dimension(:)
	allocatable :: al_list ! allocate(al_list(b)) 可以先声明，后确定内存中数组的长度
end module constant

!calculate.f90
module calculate
	use constant								! 模块的调用
	integer :: e, f
	contains
	subroutine add(add1, add2, res)				! 函数的定义
		real, INTENT(IN) :: add1, add2			! 输入的参数
		real, INTENT(OUT) :: res				! 输出的参数
		res = add1 + add2
	end subroutine add
	
	subroutine choose
		if (l_list(1) > 3) then 
			a = 1
		else if (l_list(2) < 3.5) then
			a = 2
		else
			a = 3
		end if
		
		! loop
		do a = 1, 7
			! 代表着a是从1开始到7结束，进行循环
			exit
		end do
		
		! 向屏幕打印字符
		print *, a, l_list
	end subroutine choose
	
	! 向文本中写入东西
	subroutine output
		open(unit=3, file='out.log')
		write(3, *) l_list				! 向3位置的文件中写入l_list
		close(3)
	end subroutine output
end module calculate
```

常用的算术指令：

`+ - * /` 基本算术指令

`**`乘方，比如说`3 ** 8`是三的八次幂

`mod(8, 3)`意思是8关于3取余数

`log(3)`自然底数的对数

`exp(8)`指数运算

