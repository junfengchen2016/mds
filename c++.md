# c++
## c++11
* 字符串和数值类型的转换
```
to_string
stoi/stol/stoul/stoll/stoull/stof/stod/stold
```
* random_device
```c++
std::random_device rd;

int randint = rd();
```
* std::chrono时间相关
```c++
std::chrono::duration<double> duration; //时间间隔
std::this_thread::sleep_for(duration); //sleep
LOG(INFO) << "duration is " << duration.count() << std::endl;
std::chrono::microseconds  //微秒
std::chrono::seconds //秒
end = std::chrono::system_clock::now(); //获取当前时间
```
* lambda
```c++
int a = 1, b = 2;
auto multi = [](int a, int b){
    b = a + a + a;
    return a + b;
};
 
LOG(INFO) << "by lambda: " << multi(a, b);
```