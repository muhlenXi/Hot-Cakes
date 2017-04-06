### 基本数据类型

####  char & unsigned char
	
	   typedef	__signed char		int8_t;
	   typedef unsigned char uint8_t;
	   
| int8_t | 1 byte  | 8 bit  | -128 ~ 127 |
| ---------- |:--------:| :------:| --------:|
| uint8_t | 1 byte  | 8 bit  | 0 ~ 255 |
	   
####  short & unsigned short	   
	   
		typedef	short			 int16_t;
		typedef unsigned short  uint16_t;
		
| int16_t  | 2 byte  | 16 bit  | -32768 ~ 32767 |
| ---------- |:--------:| :------:| --------:|
| uint16_t | 2 byte  | 16 bit  | 0 ~ 65535 |
		
####  int & unsigned int
	
		typedef	int			    int32_t;
		typedef unsigned int    uint32_t;

| int32_t  | 4 byte  | 32 bit  | -2^31 ~ 2^31-1 |
| ---------- |:--------:| :------:| --------:|
| uint32_t | 4 byte  | 32 bit  | 0 ~ 2^32-1 |
		
####  long long & unsigned long long

		typedef	long long		 int64_t;
		typedef unsigned long long uint64_t;

| int64_t  | 8 byte  | 64 bit  | -2^63 ~ 2^63-1 |
| ---------- |:--------:| :------:| --------:|
| uint64_t | 8 byte  | 64 bit  | 0 ~ 2^64-1 |


####  NSInteger & NSUInteger

	typedef long NSInteger;
	typedef unsigned long NSUInteger;