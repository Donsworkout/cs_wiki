### Chapter 9 Exercise

9.1 논리 메모리와 물리 메모리 차이

1) 논리적인 주소는 실제 존재하는 주소가 아니라, 논리 레벨에서 존재하는 주소이고 물리 메모리는 실제 메모리상의 물리 주소이다.

2) 논리적 주소는 CPU에 의해 생성되고, 물리적 주소는 MMU에 의해 논리 주소로 변환된다.

9.2 페이지 주소는 왜 2의 제곱형태 일까?

메모리 페이징은 페이지 + 오프셋으로 위치를 구별하는데, 비트  자리수 기준으로 2의 배수이므로 비트로 계산을 할 수 있어 편리함

9.3 프로그램이 코드와 데이터 영역으로 나뉜다고 할 때, 이를 CPU로 가져오기 위하여 base-limit 레지스터는 한쌍을 준다. 이 레지스터는 읽기 전용이므로 다른 사용자 간 프로그램을 공유할 수 있다. 장단점은?

서로 다른 사용자는 코드와 데이터를 공유할 수 있으며, 코드를 재 사용 가능하기에 메모리를 절약할 수 있다. 따라서 다른 유저와 공유가 가능하며, 프로그램에 변화가 있어도 코드를 복사할 필요가 없다. 

유일한 단점은 코드와 데이터를 분리해야 한다는 것인데, 이것은 보통 컴파일러 생성 코드로 해결될 수 있다.

9.4 논리 메모리가 64 pages 이고 각 1,024 word 로 이루어 지고 32비트 프레임들에 맵핑된다고 하자.

a. How many bits are there in the logical address?

2^6 x 2^10 = 2^16 16비트

b. How many bits are there in the physical address?

2^5 x 2^10 = 2^15 15비트

9.5 페이지 테이블의 두 항목이 메모리에 있는 동일한 페이지 프레임을 가리키도록 허용하는 효과는 무엇인가? 많은 양의 메모리를 한 장소에서 다른 장소로 복사하는 데 필요한 시간을 줄이기 위해 이 효과를 어떻게 사용할 수 있는지 설명하십시오. 한 페이지에서 일부 바이트를 업데이트하면 다른 페이지에 어떤 영향을 줍니까?

페이지 테이블의 두 항목이 메모리의 동일한 페이지 프레임을 가리키도록 허용함으로써 사용자는 코드와 데이터를 공유할 수 있다. 코드가 다시 입력되면 텍스트 편집기, 컴파일러, 데이터베이스 시스템과 같은 대형 프로그램을 공유하여 많은 메모리 공간을 절약할 수 있다.

그러나 데이터를 다른 쪽에서 수정하여 문제가 생길 수 있다.

**inverted page table은 실제 사상된 프레임의 주소만 매핑**

so 역 페이지 테이블에서 엔트리의 갯수 =  
논리적 주소 공간 / 페이지 크기

9.8 OS가 21bit 가상 메모리 주소를 가지고 있고, 물리 주소는 16bit 를 가지고 있다. 그리고 페이지 사이즈는 2KB (1024byte * 2)  이다. 아래의 각 엔트리의 개수는 몇개일까?

a. A conventional, single-level page table
2^21 / 2^11 = 2^10

b. An inverted page table
2^16 / 2^11 =  2^5

OS 물리 메모리 최대는?  
2^16 = 65536 (64-KB)

9.9 Consider a logical address space of 256 pages with a 4-KB page size, mapped onto a physical memory of 64 frames.

a. How many bits are required in the logical address?

2^8 x 2^12 = 2^20 (20bits)

b. How many bits are required in the physical address?

2^6 x 2^12 = 2^18 (18bits)

9.10 Consider a computer system with a 32-bit logical address and 4-KB page size. The system supports up to 512 MB of physical memory. How many entries are there in each of the following?

a. A conventional, single-level page table

2^8 x 2^12 = 2^20 entries

b. An inverted page table

512000 / 4 =  128000 entries