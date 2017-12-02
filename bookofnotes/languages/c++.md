# c++

## Timing (c++11)

```c++
    #include <chrono>
    #include <thread>

    const auto start_time = std::chrono::system_clock::now();

    const auto wait_time = std::chrono::milliseconds(40);

    const auto ms_from_start = std::chrono::duration_cast<std::chrono::milliseconds>(std::chrono::system_clock::now() - start_time);

    // next line gets the milliseconds for epoch ~~ in theory from 1970-01-01 00:00:00
    const auto milliseconds_from_epoc = std::chrono::duration_cast<std::chrono::milliseconds>(std::chrono::system_clock::now().time_since_epoch()).count();

    const auto usable_value = ms_from_start.count();    

    std::this_thread::sleep_for(std::chrono::seconds(1));
    
    std::this_thread::sleep_for(wait_time);
```

## Old

`popen` doesn't use `PATH`.

