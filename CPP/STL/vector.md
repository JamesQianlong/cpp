# vector定义
```c++
template<typename _Tp, typename _Alloc = std::allocator<_Tp> >
    class vector : protected _Vector_base<_Tp, _Alloc>
```
可以看出，vector有一个基类_Vector_base
```c++
template<typename _Tp, typename _Alloc>
    struct _Vector_base
    {
      typedef typename __gnu_cxx::__alloc_traits<_Alloc>::template
    rebind<_Tp>::other _Tp_alloc_type;
      typedef typename __gnu_cxx::__alloc_traits<_Tp_alloc_type>::pointer
           pointer;
      struct _Vector_impl
      : public _Tp_alloc_type
      {
    pointer _M_start;//容器开始位置
    pointer _M_finish;//容器结束位置
    pointer _M_end_of_storage;//容器所申请的动态内存最后一个位置的下一个位置

    _Vector_impl()
    : _Tp_alloc_type(), _M_start(), _M_finish(), _M_end_of_storage()
    { }
    ...  //部分代码没截取的以省略号代替，后续同理
      };
    public:
      typedef _Alloc allocator_type;
      ...//此处省略部分源代码
      _Vector_base()
      : _M_impl() { }
        _Vector_base(size_t __n)
      : _M_impl()
      { _M_create_storage(__n); }
      ...//此处省略部分源代码
    public:
      _Vector_impl _M_impl;
          pointer
      _M_allocate(size_t __n)
      {
    typedef __gnu_cxx::__alloc_traits<_Tp_alloc_type> _Tr;
    return __n != 0 ? _Tr::allocate(_M_impl, __n) : pointer();
      }

      void
      _M_deallocate(pointer __p, size_t __n)
      {
    typedef __gnu_cxx::__alloc_traits<_Tp_alloc_type> _Tr;
    if (__p)
      _Tr::deallocate(_M_impl, __p, __n);
      }

    private:
      void
      _M_create_storage(size_t __n)
      {
    this->_M_impl._M_start = this->_M_allocate(__n);
    this->_M_impl._M_finish = this->_M_impl._M_start;
    this->_M_impl._M_end_of_storage = this->_M_impl._M_start + __n;
      }
    };
```
