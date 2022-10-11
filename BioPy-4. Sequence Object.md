# 4. Sequence 객체 다루기
**_객체: 속성(객체의 특성)과 메서드(객체의 행동; Method)로 구성_**

4-1. TATA Box
 - 어떤 프로모터 서열에서도 잘 보존되어 있으며, 어떤한 종(Species)에서도 서열 정보 동일
  ![TATA Box](https://i1.wp.com/www.differencebetween.com/wp-content/uploads/2020/06/Difference-Between-TATA-and-CAAT-Box_1.png?w=635&ssl=1)  
  **출처: https://www.differencebetween.com/difference-between-tata-and-caat-box/**

 - 진핵 생물(Eukaryote)의 전사(Transcript)는 프로모터(Promoter)에 RNA Polymerase가 붙어 진행
   - Transcript에 필요한 단백질
     - Transcription factors
     - TBP(TATA Box BInding Protein)
     - RNA Polymerase

4-2. Sequence 객체
 - 문자열로 된 DNA, RNA, 단백질의 서열 정보 포함
 - Sequence 객체 생성
  ```python
  from Bio.Seq import Seq #Bio.Seq의 Seq 모듈 호출

  tatabox_seq = Seq("tataaaggc")
  print(tatabox_seq)
  >>> "tataaaggc" #TATA Box 서열 출력
  print(type(tatabox_seq))
  >>> <calss 'Bio.Seq.Seq'> #TATA Box 서열이 포함된 문자열의 타입 출력
  ```
 - Sequence 객체의 메서드 종류
    |메서드 이름|설명|
    |---|---|
    |count( )|괄호 안에 들어간 문자의 개수를 겹치지 않게 반환|
    |lower( )|Sequence 문자열을 소문자로 반환|
    |upper( )|Sequence 문자열을 대문자로 반환|
    |split( )|Sequence 문자열을 문자 기준으로 구분<br>문자가 없다면 공백을 기준(생물학 서열에서 확인 불가)|
    |strip( )|양끝 공백, 엔터 등의 문자 제거|
    |startwith( )|Sequence 객체의 서열이 지정 문자열로 시작하는지<br>참,거짓으로 판단|
    |transcribe( )|DNA Sequence -> RNA Sequence|
    |translate( )|DNA, RNA Sequence -> Protein Sequence|
    |complement( )|Sequence 객체가 가진 서열의 상보적 서열 반환|
    |reverse_complement( )|Sequence 객체가 가진 서열의 역상보적 서열 반환|
    
    ```python
    from Bio.Seq import Seq #Bio.Seq의 Seq 모듈 호출
    tatabox_seq = Seq("tataaaggc") #TATA Box 서열 입력

    com_tatabox_seq = Seq.complement(tatabox_seq)
    >>> atatttccg #TATA Box 서열의 상보적 서열 출력

    re_tatabox_seq = Seq.reverse_complement("tataaaggc")
    >>> gcctttata #TATA Box 서열의 역상보적 서열 출력
    ```

4-3. Sequence 객체 다루기
 - 여러 method를 사용해서 Sequence 객체에서 원하는 정보의 추출이 가능
    ```python
    from Bio.Seq import Seq #Bio.Seq의 Seq 모듈 호출
    DNA_Seq = Seq("attacgc")

    #DNA 서열에서 "a" 염기의 수 출력
    count_a = DNA_Seq.count("a")
    >>> 2

    #GC-contents(%) 구하기
    count_g = DNA_Seq.count("g")
    count_c = DNA_Seq.count("C")
    gc_con = (count_g + count_c) / len(DNA-Seq) * 100
    print(gc_con)
    >>> 14.285714285714285 #대략 14.3%
    ```
   - GC-contents(%)를 구하는 식
     - GC-contents(%) = $(C 염기 수 + G 염기 수)\over (전체 염기수)$ x 100(%)
 - 대소문자의 변경
    ```python
    from Bio.Seq import Seq #Bio.Seq의 Seq 모듈 호출
    tatabox_Seq = Seq("tataaaggcAATATGC")

    print(tatabox_Seq.upper())
    >>> TATAAAGGCAATATGC #모두 대문자
    print(tatabox_Seq.lower())
    >>> tataaaggcaatatgc #모두 소문자
    ```
 - Sequence 객체의 전사, 번역
   - BioPython을 사용하여 DNA 서열에서 mRNA로의 전사(Transcribe) 후 단백질로의 번역(Translate) 진행
   - 코드 가닥(Coding Strand)과 주형 가닥(Template Strand)에서의 전사, 번역 확인 가능
     - 코드 가닥: 5'-염기 서열-3'(5' -> 3' 방향으로 전사 진행)
     - 주형 가닥: 3'-염기 서열-5'(3' -> 5' 방향으로 전사 진행)
     - mRNA: 주형 가닥으로부터 전사되어 나온 결과물 (5'-mRNA 서열-3')
     - 번역: 5'-mRNA 서열-3' 을 통해 나오는 단백질 결과물
     - 번역은 종결 코돈에 의해 번역 종료
      ```python
      from Bio.Seq import Seq #Bio.Seq의 Seq 모듈 호출
      DNA = Seq("ATGCAGTAG") #코드 가닥: 5'-서열-3'
      mRNA = DNA.transcribe() #3'-주형 가닥-5' 변환 후 mRNA 전사
      ptn = DNA.translate() #mRNA를 사용하여 단백질로 번역
      print(mRNA)
      >>> AUGCAGUAG #코드 가닥으로부터 전사된 mRNA 서열
      print(ptn)
      >>> MQ* #mRNA로부터 번역된 단백질 서열, *는 종결 코돈에 의해 종결된 부분
      ```
   - 종결 코돈이 중간에 있을 때, 번역 종료
      ```python
      from Bio.Seq import Seq #Bio.Seq의 Seq 모듈 호출
      #mRNA = "AUGAACUAAGUUUAGAAU"

      mRNA = Seq("AUGAACUAAGUUUAGAAU")
      ptn = mRNA.translate()
      print(ptn)
      >>> MN*V*N #종결 코돈이 중간에 위치
      ptn = mRNA.translate(to_stop=True) #to_stop=True 값을 사용하여 첫번째 종결 코돈에서 번역 종료
      print(ptn)
      >>> MN
      ```
   - 종결 코돈을 기준으로 서열 나누기
      ```python
      #종결 코돈을 기준으로 단백질을 나눌 때, .split() 함수 사용

      from Bio.Seq import Seq #Bio.Seq의 Seq 모듈 호출

      mRNA = Seq("AUGAACUAAGUUUAGAAU")
      ptn = mRNA.translate()
      for seq in ptn.split("*"):
        print(seq)
      >>> MN V N
      ```
   - DNA Sequence를 사용한 상보적, 역상보적 서열 생성
      ```python
      #상보적 관계의 두 서열
      #5'-TATAAAGGCAATATGCAGTAG-3'
      #3'-ATATTTCCGTTATACGTCATC-5'
      
      #python을 사용    
      seq = "TATAAAGGCAATATGCAGTAG"
      comp_dic = {'A':'T', 'C':'G', 'G':'C', 'T':'A'} #상보적 염기를 서로 {'키':'값'}의 딕셔너리 형태로 생성
      comp_seq = "" #상보적인 서열을 차례로 추가하기 위한 빈 변수 생성
      for s in seq: #seq에 저장된 서열을 염기 하나씩 읽을 수 있게 for문 사용
        comp_seq += comp_dic[s] #comp_seq 변수에 comp_dic에서 설정한 키:값에 의해 차례로 추가
      revcomp_seq = comp_seq[::-1] #생성된 comp_seq 서열을 뒤부터 차례로 정렬
      print(comp_seq)
      >>> ATATTTCCGTTATACGTCATC
      print(revcomp_seq)
      >>> CTACTGCATATTGCCTTTATA

      #Bio Python 사용
      from Bio.Seq import Seq #Bio.Seq의 Seq 모듈 호출
      seq = Seq("TATAAAGGCAATATGCAGTAG")
      comp_seq = seq.complement() #seq 서열의 상보적 서열
      revcomp_seq = seq.reverse_complement() #seq 서열의 역상보적 서열
      print(comp_seq)
      >>> ATATTTCCGTTATACGTCATC
      print(revcomp_seq)
      >>> CTACTGCATATTGCCTTTATA
      ```
   - 코돈 테이블
      ```python
      #기본 코돈 테이블 출력
      from Bio.Data import CodonTable #Bio.Data의 CodonTable 모듈 호출

      condon_T = CodonTable.unambiguous_dna_by_name["Standard"] #기본적인 코돈 테이블 출력
      print(codon_T)
      >>>
      Table 1 Standard, SGC0

        |  T      |  C      |  A      |  G      |
      --+---------+---------+---------+---------+--
      T | TTT F   | TCT S   | TAT Y   | TGT C   | T
      T | TTC F   | TCC S   | TAC Y   | TGC C   | C
      T | TTA L   | TCA S   | TAA Stop| TGA Stop| A
      T | TTG L(s)| TCG S   | TAG Stop| TGG W   | G
      --+---------+---------+---------+---------+--
      C | CTT L   | CCT P   | CAT H   | CGT R   | T
      C | CTC L   | CCC P   | CAC H   | CGC R   | C
      C | CTA L   | CCA P   | CAA Q   | CGA R   | A
      C | CTG L(s)| CCG P   | CAG Q   | CGG R   | G
      --+---------+---------+---------+---------+--
      A | ATT I   | ACT T   | AAT N   | AGT S   | T
      A | ATC I   | ACC T   | AAC N   | AGC S   | C
      A | ATA I   | ACA T   | AAA K   | AGA R   | A
      A | ATG M(s)| ACG T   | AAG K   | AGG R   | G
      --+---------+---------+---------+---------+--
      G | GTT V   | GCT A   | GAT D   | GGT G   | T
      G | GTC V   | GCC A   | GAC D   | GGC G   | C
      G | GTA V   | GCA A   | GAA E   | GGA G   | A
      G | GTG V   | GCG A   | GAG E   | GGG G   | G
      --+---------+---------+---------+---------+--

      #미토콘드리아의 코돈 테이블 출력
      codon_T_Mito = CodonTable.unambiguous_dna_by_name["Vertebrate Mitochondrial"]
      print(codon_T_Mito)
      >>>
      Table 2 Vertebrate Mitochondrial, SGC1

        |  T      |  C      |  A      |  G      |
      --+---------+---------+---------+---------+--
      T | TTT F   | TCT S   | TAT Y   | TGT C   | T
      T | TTC F   | TCC S   | TAC Y   | TGC C   | C
      T | TTA L   | TCA S   | TAA Stop| TGA W   | A
      T | TTG L   | TCG S   | TAG Stop| TGG W   | G
      --+---------+---------+---------+---------+--
      C | CTT L   | CCT P   | CAT H   | CGT R   | T
      C | CTC L   | CCC P   | CAC H   | CGC R   | C
      C | CTA L   | CCA P   | CAA Q   | CGA R   | A
      C | CTG L   | CCG P   | CAG Q   | CGG R   | G
      --+---------+---------+---------+---------+--
      A | ATT I(s)| ACT T   | AAT N   | AGT S   | T
      A | ATC I(s)| ACC T   | AAC N   | AGC S   | C
      A | ATA M(s)| ACA T   | AAA K   | AGA Stop| A
      A | ATG M(s)| ACG T   | AAG K   | AGG Stop| G
      --+---------+---------+---------+---------+--
      G | GTT V   | GCT A   | GAT D   | GGT G   | T
      G | GTC V   | GCC A   | GAC D   | GGC G   | C
      G | GTA V   | GCA A   | GAA E   | GGA G   | A
      G | GTG V(s)| GCG A   | GAG E   | GGG G   | G
      --+---------+---------+---------+---------+--
      ```
   - Sequence 객체에서 ORF(Open Reading Frame) 찾기
     - ORF: Open Reading Frame, mRNA로 전사되어 단백질이 될 가능성이 있는 염기서열
     - 변화 조건
       - 어떤 ATG(AUG) 서열로 시작하는가
       - 3개씩 묶인 염기서열을 코돈으로 묶을 때, 어떻게 묶는가
       - DNA의 반대 Strand에서 전사되는지에 따라 시작 코돈과 종결 코돈에 변화가 생기는가
     - ORF 중 종결 코돈 전에 전사가 종료되면, 번역 과정에서 불안정한 단백질 생성
    ```python
    #ORF
    #TATAAAGGCAATATGCAGTAG -> 전체에서 일부에 해당하는 서열 사용
    from Bio.Seq import Seq #Bio.Seq에서 Seq 모듈 호출

    tata_seq = Seq("tataaaggcAATATGCAGTAG")
    start_idx = tata_seq.find("ATG") #서열에서 시작 코돈(ATG) 탐색
    end_idx = tata_seq.find("TAG", start_idx) #편의 상 예문 서열에 사용된 종결 코돈(TAG) 사용
    orf = tata_seq[start_idx:end_idx+3] #파이썬 문자열 슬라이싱과 같은 방법으로 슬라이싱
    print(orf)
    >>> ATGCAGTAG
    ```

4-4. Bio.SeqUtils 모듈 활용
 - Bio.SeqUtils 모듈을 활용하여 GC-contents(%), 서열의 무게 계산, 유전 서열에서 확인 가능한 모든 아미노산 서열 정리 등의 메서드 함수 포함
   - GC-contents(%) 계산
    ```python
    from Bio.Seq import Seq #Bio.Seq의 Seq 모듈 호출
    from Bio.SeqUtils import GC #Bio.SeqUtils의 GC 모듈 호출

    exon_seq = Seq("ATGCAGTAG")
    gc_con = GC(exon_seq)
    print(gc_con)
    >>> 44.44444444444444
    ```
   - 서열의 무게 계산
    ```python
    from Bio.Seq import Seq #Bio.Seq의 Seq 모듈 호출
    from Bio.SeqUtils import molecular_weight #Bio.SeqUtils의 molecular_weight 모듈 호출

    seq = Seq("ATGCAGTAG")
    print(molecular_weight(seq, seq_type='DNA')) #서열의 DNA 무게 출력
    >>> 2842.8206999999993
    print(molecular_weight(seq, seq_type='protein')) #서열의 단백질 무게 출력
    >>> 707.7536

    RNA_weight = seq.transcribe()
    print(molecular_weight(RNA_weight, seq_type='RNA')) #서열의 RNA 무게 출력
    >>> 2958.7621
    #seq_type='RNA'를 사용하기 위해서는 서열에 'T' 대신 'U'가 있어야 계산 가능
    ```
   - 가능한 모든 번역 출력
    ```python
    from Bio.Seq import Seq
    from Bio.SeqUtils import six_frame_translations #Bio.SeqUtils의 six_frame_translations 모듈 호출

    seq = Seq("ATGCCTTGAAATGTATAG")
    print(six_frame_translations(seq))
    >>>
    GC_Frame: a:6 t:6 g:4 c:2 
    Sequence: atgccttgaaatgtatag, 18 nt, 33.33 %GC


    1/1
      A  L  K  C  I
    C  L  E  M  Y
    M  P  *  N  V  *
    atgccttgaaatgtatag   33 %
    tacggaactttacatatc
    G  Q  F  T  Y 
    H  R  S  I  Y  L
      A  K  F  H  I
    ```
   - DNA 서열의 Tm 계산
     - Tm: DNA의 이중 나선에 열이 가해져 이중 나선의 절반이 단일 나선이 될 때의 온도 
     - GC %가 높을수록 Tm 값 증가 (GC 간 결합[3중 결합]의 힘이 AT 결합[2중 결합]보다 강하기 때문)
    ```python
    from Bio.Seq import Seq
    from Bio.SeqUtils import MeltingTemp as mt #mt로 Bio.SeqUtils의 MeltingTmp 모듈 사용

    seq = Seq("ATGCCTTGAAATGTATAG")
    print(mt.Tm_Wallace(seq))
    >>> 56.0
    ```
   - 아미노산 서열의 약자와 기호 간 변환
     - seq1: 아미노산 서열 기호를 약자로 변환
     - seq3: 아미노산 서열 약자를 기호로 변환
    ```python
    from Bio.SeqUtils import seq1 #Bio.SeqUtils의 seq1 모듈 호출
    amino = "LeuLysMetValIleThrTrpPhe"
    print(seq1(amino))
    >>> LKMVITWF

    from Bio.SeqUtils import seq3 #Bio.SeqUtils의 seq3 모듈 호출
    amino_S = "LKMVITWF"
    print(seq3(amino_S))
    >>> LeuLysMetValIleThrTrpPhe
    ```