# vector源码

```c++
#include <compare>
#include <initializer_list>
 
namespace std {
  // class template vector
  template<class T, class Allocator = allocator<T>> class vector;
 
  template<class T, class Allocator>
    constexpr bool operator==(const vector<T, Allocator>& x,
                              const vector<T, Allocator>& y);
  template< class T, class Alloc > 
    constexpr bool operator!=( const std::vector<T, Alloc>& lhs,
                 const std::vector<T, Alloc>& rhs );   
  template< class T, class Alloc > 
    constexpr bool operator<=( const std::vector<T, Alloc>& lhs,
                 const std::vector<T, Alloc>& rhs );                          
  c++20使用 operator<=>和operator==合成<、<=、>、>=、!=
  template<class T, class Allocator>
    constexpr __synth_three_way_result<T> operator<=>(const vector<T, Allocator>& x,
                                                    const vector<T, Allocator>& y); //三路比较运算符 c++ 20
 
  template<class T, class Allocator>
    constexpr void swap(vector<T, Allocator>& x, vector<T, Allocator>& y)
      noexcept(noexcept(x.swap(y)));
 
  // erasure
  template<class T, class Allocator, class U = T>
    constexpr typename vector<T, Allocator>::size_type
      erase(vector<T, Allocator>& c, const U& value);
  template<class T, class Allocator, class Predicate>
    constexpr typename vector<T, Allocator>::size_type
      erase_if(vector<T, Allocator>& c, Predicate pred);
 
  namespace pmr {
    template<class T>
      using vector = std::vector<T, polymorphic_allocator<T>>;
  }
 
  // specialization of vector for bool
  // partial class template specialization vector<bool, Allocator>
  template<class Allocator>
    class vector<bool, Allocator>;
 
  template<class T>
    constexpr bool __is_vector_bool_reference = /* see description */; // exposition only
 
  // hash support
  template<class T> struct hash;
  template<class Allocator> struct hash<vector<bool, Allocator>>;
 
  // formatter specialization for vector<bool>
  template<class T, class CharT> requires __is_vector_bool_reference<T>
    struct formatter<T, CharT>;
}
```
 
类模板  

```c++
namespace std {
  template<class T, class Allocator = allocator<T>>
  class vector {
  public:
    // types
    using value_type             = T; //值类型
    using allocator_type         = Allocator; //分配器
    using pointer                = typename allocator_traits<Allocator>::pointer;    
    using const_pointer          = typename allocator_traits<Allocator>::const_pointer;
    using reference              = value_type&;
    using const_reference        = const value_type&;
    using size_type              = /* implementation-defined */;
    using difference_type        = /* implementation-defined */;
    using iterator               = /* implementation-defined */;
    using const_iterator         = /* implementation-defined */;
    using reverse_iterator       = std::reverse_iterator<iterator>;
    using const_reverse_iterator = std::reverse_iterator<const_iterator>;
 
    // construct/copy/destroy
    constexpr vector() noexcept(noexcept(Allocator())) : vector(Allocator()) { }
    constexpr explicit vector(const Allocator&) noexcept;
    constexpr explicit vector(size_type n, const Allocator& = Allocator());
    constexpr vector(size_type n, const T& value, const Allocator& = Allocator());
    template<class InputIt>
      constexpr vector(InputIt first, InputIt last, const Allocator& = Allocator());
    template<container-compatible-range<T> R>
      constexpr vector(from_range_t, R&& rg, const Allocator& = Allocator());
    constexpr vector(const vector& x);
    constexpr vector(vector&&) noexcept;
    constexpr vector(const vector&, const type_identity_t<Allocator>&);
    constexpr vector(vector&&, const type_identity_t<Allocator>&);
    constexpr vector(initializer_list<T>, const Allocator& = Allocator());
    constexpr ~vector();
    constexpr vector& operator=(const vector& x);
    constexpr vector& operator=(vector&& x)
      noexcept(
        allocator_traits<Allocator>::propagate_on_container_move_assignment::value ||
        allocator_traits<Allocator>::is_always_equal::value
      );
    constexpr vector& operator=(initializer_list<T>);
    template<class InputIt>
      constexpr void assign(InputIt first, InputIt last);
    template<container-compatible-range<T> R>
      constexpr void assign_range(R&& rg);
    constexpr void assign(size_type n, const T& u);
    constexpr void assign(initializer_list<T>);
    constexpr allocator_type get_allocator() const noexcept;
 
    // iterators
    constexpr iterator               begin() noexcept;
    constexpr const_iterator         begin() const noexcept; //C++ 14
    constexpr iterator               end() noexcept;
    constexpr const_iterator         end() const noexcept; //C++ 14
    constexpr reverse_iterator       rbegin() noexcept; //C++ 14
    constexpr const_reverse_iterator rbegin() const noexcept; //C++ 14
    constexpr reverse_iterator       rend() noexcept;    //C++ 14
    constexpr const_reverse_iterator rend() const noexcept;      //C++ 14
 
    constexpr const_iterator         cbegin() const noexcept;
    constexpr const_iterator         cend() const noexcept;
    constexpr const_reverse_iterator crbegin() const noexcept;
    constexpr const_reverse_iterator crend() const noexcept;
 
    // capacity
    constexpr bool empty() const noexcept;
    constexpr size_type size() const noexcept;
    constexpr size_type max_size() const noexcept;
    constexpr size_type capacity() const noexcept;
    constexpr void      resize(size_type sz);
    constexpr void      resize(size_type sz, const T& c);
    constexpr void      reserve(size_type n);
    constexpr void      shrink_to_fit();
 
    // element access
    constexpr reference       operator[](size_type n);
    constexpr const_reference operator[](size_type n) const;
    constexpr const_reference at(size_type n) const;
    constexpr reference       at(size_type n);
    constexpr reference       front();
    constexpr const_reference front() const;
    constexpr reference       back();
    constexpr const_reference back() const;
 
    // data access
    constexpr T*       data() noexcept;
    constexpr const T* data() const noexcept;
 
    // modifiers
    template<class... Args> constexpr reference emplace_back(Args&&... args);
    constexpr void push_back(const T& x);
    constexpr void push_back(T&& x);
    template<container-compatible-range<T> R>
      constexpr void append_range(R&& rg);
    constexpr void pop_back();
 
    template<class... Args> constexpr iterator emplace(const_iterator position,
                                                       Args&&... args);
    constexpr iterator insert(const_iterator position, const T& x);
    constexpr iterator insert(const_iterator position, T&& x);
    constexpr iterator insert(const_iterator position, size_type n, const T& x);
    template<class InputIt>
      constexpr iterator insert(const_iterator position,
                                InputIt first, InputIt last);
    template<container-compatible-range<T> R>
      constexpr iterator insert_range(const_iterator position, R&& rg);
    constexpr iterator insert(const_iterator position, initializer_list<T> il);
    constexpr iterator erase(const_iterator position);
    constexpr iterator erase(const_iterator first, const_iterator last);
    constexpr void     swap(vector&)
      noexcept(allocator_traits<Allocator>::propagate_on_container_swap::value ||
               allocator_traits<Allocator>::is_always_equal::value);
    constexpr void     clear() noexcept;
  };
 
  template<class InputIt, class Allocator = allocator<__iter_value_type<InputIt>>>
    vector(InputIt, InputIt, Allocator = Allocator())
      -> vector<__iter_value_type<InputIt>, Allocator>;
 
  template<ranges::input_range R, class Allocator = allocator<ranges::range_value_t<R>>>
    vector(from_range_t, R&&, Allocator = Allocator())
      -> vector<ranges::range_value_t<R>, Allocator>;
}
```
