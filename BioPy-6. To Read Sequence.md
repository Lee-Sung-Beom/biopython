# 6. FASTA, FASTQ, Genbank 파일의 Sequence 읽기
6-1. SeqIO 모듈로 FASTA 파일 읽기
 - SeqIO 모듈의 .parse( ) / .read( ) method를 사용하여 파일 내 Sequence 읽기 가능
   - 예시를 위해 NCBI에서 FASTA 파일 다운로드
   - 두 번째, 세 번째 서열은 하나의 파일에 포함
   > AF501235.1 Influenzavirus A (A/duck/Shanghai/1/2000) hemagglutinin gene, complete cds
ATGGAGAAAATAGTGCTTCTTCTTGCAATAGTCAGTCTTGTTAAAAGTGATCAGATTTGCATTGGTTACC
ATGCAAACAACTCGACAGAGCAGGTTGACACAATAATGGAAAAGAACGTTACTGTTACACATGCCCAAGA
   
   > MH464856.1 Hepatitis B virus isolate MA134, complete genome
TTCCACAACATTCCACCAAGCTCTGCAGGATCCCAGAGTAAGAGGCCTGTATTTTCCTGCTGGTGGCTCC
AGTTCCGGAACAGTGAACCCTGTTCCGACTACTGCCTCACTCATCTCGTCAATCTTCTCGAGGATTGGGG
ACCCTGCACCGAACATGGAAAGCATCACATCAGGATTCCTAGGACCCCTGCTCGCGTTACAGGCGGGGTT
   
   > CP002925.1 Streptococcus pseudopneumoniae IS7493, complete genome
TTGAAAGAAAAACAATTTTGGAATCGTATATTAGAATTTGCTCAAGAAAGACTGACTCGATCCATGTATG
ATTTCTATGCTATTCAAGCTGAACTCATCAAGGTAGAGGAAAATGTTGCCACTATATTTTTACCACGATC

 - SeqIO.parse( )를 사용한 파일 읽기
    ```python
    #서열이 1개만 들어있는 fasta 파일 읽기
    from Bio import SeqIO

    seq1 = SeqIO.parse("Influenzavirus A.fasta", "fasta")
    print(type(seq1))
    print("") #줄 구분을 위한 출력값
    for s in seq1:
        print(type(s))
        print(s)
        print("") #줄 구분을 위한 출력값
    >>>
    <class 'Bio.SeqIO.FastaIO.FastaIterator'>

    <class 'Bio.SeqRecord.SeqRecord'>
    ID: AF501235.1
    Name: AF501235.1
    Description: AF501235.1 Influenzavirus A (A/duck/Shanghai/1/2000) hemagglutinin gene, complete cds
    Number of features: 0
    Seq('ATGGAGAAAATAGTGCTTCTTCTTGCAATAGTCAGTCTTGTTAAAAGTGATCAG...AGT')
    ```
     - .parse( ) method 인자
       - 파일 이름
       - 파일 종류
     - .parse( ) method가 읽을 수 있는 파일 포맷(일부)
       - clustal: Multiple Alignment에 사용되는 포맷
       - fasta: 1개 이상의 레코드가 들어가는 염기 서열 파일 포맷
       - fastq: FASTA 파일에 quality가 포함된 파일 포맷
       - genbank: 염기 서열 및 해당 유전자의 메타 데이터 포함하는 파일 포맷
    ```python
    #서열이 2개가 들어있는 fasta 파일 읽기
    from Bio import SeqIO

    seq2 = SeqIO.parse("Hepatitis, Streptococcus.fasta", "fasta")
    print(type(seq2))
    print("")
    for i in seq2:
        print(type(i))
        print(i)
    >>>
    <class 'Bio.SeqIO.FastaIO.FastaIterator'>

    <class 'Bio.SeqRecord.SeqRecord'>
    ID: MH464856.1
    Name: MH464856.1
    Description: MH464856.1 Hepatitis B virus isolate MA134, complete genome
    Number of features: 0
    Seq('TTCCACAACATTCCACCAAGCTCTGCAGGATCCCAGAGTAAGAGGCCTGTATTT...GAA')
    <class 'Bio.SeqRecord.SeqRecord'>
    ID: CP002925.1
    Name: CP002925.1
    Description: CP002925.1 Streptococcus pseudopneumoniae IS7493, complete genome
    Number of features: 0
    Seq('TTGAAAGAAAAACAATTTTGGAATCGTATATTAGAATTTGCTCAAGAAAGACTG...TCT')
    #for문에 의해 2개의 서열이 순서대로 반환
    ```
 - SeqIO.read( ) 를 사용한 파일 읽기
   - SeqIO.read( ) method 인자
     - 파일 이름
     - 파일 종류
   - SeqRecord 객체로 for문을 순환하지 않음
   - SeqIO.read( ) method로는 2개 이상의 레코드가 담긴 FASTA 파일을읽지 못함
     - ValueError 발생
    ```python
    #서열이 1개만 들어있는 fasta 파일 읽기
    from Bio import SeqIO

    seq3 = SeqIO.read("Influenzavirus A.fasta", "fasta")
    print(type(seq3))
    print(seq3)
    >>>
    <class 'Bio.SeqRecord.SeqRecord'>
    ID: AF501235.1
    Name: AF501235.1
    Description: AF501235.1 Influenzavirus A (A/duck/Shanghai/1/2000) hemagglutinin gene, complete cds
    Number of features: 0
    Seq('ATGGAGAAAATAGTGCTTCTTCTTGCAATAGTCAGTCTTGTTAAAAGTGATCAG...AGT')

    #서열이 2개가 들어있는 fasta 파일 읽기
    from Bio import SeqIO

    seq4 = SeqIO.read("Hepatitis, Streptococcus.fasta", "fasta")
    print(type(seq4))
    print(seq4)
    >>>
    ---------------------------------------------------------------------------
    ValueError                                Traceback (most recent call last)
    Input In [5], in <cell line: 1>()
    ----> 1 seq4 = SeqIO.read("Hepatitis, Streptococcus.fasta", "fasta")
        2 print(type(seq4))
        3 print(seq4)

    File C:\ProgramData\Anaconda3\lib\site-packages\Bio\SeqIO\__init__.py:661, in read(handle, format, alphabet)
        659 try:
        660     next(iterator)
    --> 661     raise ValueError("More than one record found in handle")
        662 except StopIteration:
        663     pass

    ValueError: More than one record found in handle
    ```

6-2. SeqIO 모듈로 FATAQ 파일 읽기
 - FASTQ 파일은 여러개의 리드들로 구성되어 있기에 .read( ) method 사용 시 ValueError 발생
   - 예시를 위해 fastq 파일 다운로드
   > 