# Summary
There is a memory leak vulnerability in apng2git-1.7.1.

https://apng2gif.sourceforge.net/

https://github.com/zyzsdy/apng2gif

# Details
When I compiled it with ASAN, and ran apng2gif in this way:
```bash
/home/n0zom1z0/apng2gif-main/build/apng2gif ../apng_sample/Animated_PNG_example_bouncing_beach_ball.png ../tmp
```

Error occurred:
![image](https://github.com/user-attachments/assets/f5a9fc93-4141-4caf-b9c1-49ad5ee247de)

It seems that when it comes to error in saving `pAGIF` into `szOut`, the commited memory `pAGIF` will not be deleted properly.
![image](https://github.com/user-attachments/assets/747b4a06-26bc-4804-9205-d262a6aae861)


# PoC
```bash
/home/n0zom1z0/apng2gif-main/build/apng2gif ../apng_sample/Animated_PNG_example_bouncing_beach_ball.png ../tmp
```
![image](https://github.com/user-attachments/assets/f5a9fc93-4141-4caf-b9c1-49ad5ee247de)
