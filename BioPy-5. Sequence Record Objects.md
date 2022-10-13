# 5. Sequence record 객체
5-1. SeqRecord 객체
 - SeqRecord 객체: Seq 객체에 추가적인 정보들을 넣을 수 있는 객체
   - 서열 외의 서열 이름, NCBI의 ID 등의 추가 정보를 객체 안에 삽입 가능
    ```python
    from Bio.Seq import Seq
    from Bio.SeqRecord import SeqRecord #Bio.SeqRecord의 SeqRecord 모듈 호출
    seq = Seq("ATGG") #Sequence 객체 생성
    seqRecord = SeqRecord(seq) #SeqRecord 객체 생성
    print(seqRecord)
    >>>
    ID: <unknown id>
    Name: <unknown name>
    Description: <unknown description>
    Number of features: 0
    Seq('ATGG')
    ```
 - 별도의 추가 정보를 넣지 않아 추가 정보는 모두 unknown으로 표시
 - 정보 추가
    ```python
    from Bio.Seq import Seq
    from Bio.SeqRecord import SeqRecord #Bio.SeqRecord의 SeqRecord 모듈 호출
    seq = Seq("ATGG")
    seqRecord = SeqRecord(seq, id="NC_1111", name="TEST") #SeqRecord 객체 생성 시 id와 이름 추가
    print(seqRecord)
    >>>
    ID: NC_1111
    Name: TEST
    Description: <unknown description>
    Number of features: 0
    Seq('ATGG')

    #객체 생성 후 속성값 변경
    seqRecord.name = "TEST2"
    print(seqRecord)
    >>>
    ID: NC_1111
    Name: TEST2
    Description: <unknown description>
    Number of features: 0
    Seq('ATGG')
    ```

5-2. SeqRecord 객체의 속성
 - SeqRecord에서 사용할 수 있는 속성
    |속성 이름|설명|
    |---|---|
    |seq|서열|
    |id|Locus와 같은 ID|
    |name|서열의 이름으로 유전자 이름과 같은 이름|
    |description|추가 설명|
    |letter_annotation|파이썬의 Dictionary 형태로 추가 설명을 [키-값] 형태로 추가|
    |annotations|추가 설명을 파이썬의 Dictionary 형태로 추가|
    |features|서열 구간에 대한 특징|
    |dbxrefs|추가 데이터베이스에 대한 내용|

5-3. SeqRecord 객체 생성
 - 문자열로 SeqRecord 객체 생성
    ```python
    from Bio.Seq import Seq
    from Bio.SeqRecord import SeqRecord

    #SeqRecord 객체 생성
    seq = Seq("ATGC")
    seqRe = SeqRecord(seq)
    print(seqRe)
    print("-------------")

    #SeqRecord 객체에 정보 추가
    seqRe.id = "TEST_1111"
    seqRe.name = "My Gene"
    seqRe.description = "This is a description"
    seqRe.annotations["Annotation_Key_1"] = "Annotation_Value_1"
    seqRe.annotations["Annotation_Key_2"] = "Annotation_Value_2"
    print(seqRe)
    >>>
    ID: <unknown id>
    Name: <unknown name>
    Description: <unknown description>
    Number of features: 0
    Seq('ATGC')
    -------------
    ID: TEST_1111
    Name: My Gene
    Description: This is a description
    Number of features: 0
    /Annotation_Key_1=Annotation_Value_1
    /Annotation_Key_2=Annotation_Value_2
    Seq('ATGC')
    ```
 - FASTA 파일로부터 SeqRecord 객체 생성
   - 해당 예시는 E.coli의 Lac Operon 유전자를 예시로 설명
   ![Lac Operon](https://microbenotes.com/wp-content/uploads/2018/09/Inducers-and-the-Induction-of-Lac-Operon.jpg)
   **출처: https://microbenotes.com/lac-operon/**
   - NCBI에서 E.coli의 Lac Operon에 관한 FASTA 파일 다운로드 후 작업 영역으로 이동
    ```python
    from Bio import SeqIO #Bio의 SeqIO 모듈 호출

    recordTEST = SeqIO.read("J01636.1_E.coli_Lac Operon.fasta", "fasta")
    #SeqIO 모듈의 .read method를 사용하여 fasta 파일 읽기
    print(type(recordTEST))
    print(recordTEST)
    >>>
    <class 'Bio.SeqRecord.SeqRecord'>
    ID: J01636.1
    Name: J01636.1
    Description: J01636.1 E.coli lactose operon with lacI, lacZ, lacY and lacA genes
    Number of features: 0
    Seq('GACACCATCGAATGGCGCAAAACCTTTCGCGGTATGGCATGATAGCGCCCGGAA...GAC')
    ```
   - 실제 FASTA 파일 형식
     - 실제 FASTA 파일의 헤더에 포함된 ID, NAME, Description, Sequence의 내용이 추가
    >J01636.1 E.coli lactose operon with lacI, lacZ, lacY and lacA genes
    GACACCATCGAATGGCGCAAAACCTTTCGCGGTATGGCATGATAGCGCCCGGAAGAGAGTCAATTCAGGG
    TGGTGAATGTGAAACCAGTAACGTTATACGATGTCGCAGAGTATGCCGGTGTCTCTTATCAGACCGTTTC ... (후략)    
 - GenBank 파일로부터 SeqRecord 객체 생성
   - FASTA 파일로부터 SeqRecord 객체를 생성하는 방법과 동일
    ```python
    from Bio import SeqIO
    recordTESTgb = SeqIO.read("J01636.1_E.coli_Lac Operon.gb", "genbank")
    #genbank <-> gb 모두 사용 가능
    print(type(recordTESTgb))
    print(recordTESTgb)
    >>>
    <class 'Bio.SeqRecord.SeqRecord'>
    ID: J01636.1
    Name: ECOLAC
    Description: E.coli lactose operon with lacI, lacZ, lacY and lacA genes
    Number of features: 22
    /molecule_type=DNA
    /topology=linear
    /data_file_division=BCT
    /date=05-MAY-1993
    /accessions=['J01636', 'J01637', 'K01483', 'K01793']
    /sequence_version=1
    /keywords=['acetyltransferase', 'beta-D-galactosidase', 'galactosidase', 'lac operon', 'lac repressor protein', 'lacA gene', 'lacI gene', 'lacY gene', 'lacZ gene', 'lactose permease', 'mutagenesis', 'palindrome', 'promoter region', 'thiogalactoside acetyltransferase']
    /source=Escherichia coli
    ...(후략)
    ```

5-4. SeqRecord 객체 간 비교
 - == 연산자를 사용하여 True, False 값 반환
    ```python
    #파이썬 연산자를 사용한 단순 비교
    str1 = ("ATGC")
    str2 = ("ATGC")
    print(str1)
    >>> ATGC
    print(str2)
    >>> ATGC
    print(str1 == str2)
    >>> True

    #SeqRecord 객체로 비교
    from Bio.Seq import Seq
    from Bio.SeqRecord import SeqRecord

    seq1 = Seq("ATGC")
    seq2 = Seq("ATGC")
    seqRe1 = SeqRecord(seq1)
    seqRe2 = SeqRecord(seq2)
    print(seqRe1)
    print("---------")
    print(seqRe2)
    >>>
    ID: <unknown id>
    Name: <unknown name>
    Description: <unknown description>
    Number of features: 0
    Seq('ATGC')
    ---------
    ID: <unknown id>
    Name: <unknown name>
    Description: <unknown description>
    Number of features: 0
    Seq('ATGC')

    #연산자를 사용하여 SeqRecord 비교
    print(seqRe1 == seqRe2)
    >>>
    ---------------------------------------------------------------------------
    NotImplementedError                       Traceback (most recent call last)
    Input In [13], in <cell line: 1>()
    ----> 1 print(seqRe1 == seqRe2)

    File C:\ProgramData\Anaconda3\lib\site-packages\Bio\SeqRecord.py:789, in SeqRecord.__eq__(self, other)
        787 def __eq__(self, other):
        788     """Define the equal-to operand (not implemented)."""
    --> 789     raise NotImplementedError(_NO_SEQRECORD_COMPARISON)

    NotImplementedError: SeqRecord comparison is deliberately not implemented. Explicitly compare the attributes of interest.
    ```
   - SeqRecord 객체를 파이썬 연산자로 비교했을 때 오류 출력
   - SeqRecord 객체끼리 == 연산자를 사용하면 오류가 발생하도록 설계
    ```python
    #SeqRecord 객체 간 비교 방식
    print(seqRe1.seq == seqRe2.seq)
    >>> True
    ```
   - SeqRecord 객체 간 비교를 위해서는 .seq 을 사용하여 속성끼리의 비교 진행