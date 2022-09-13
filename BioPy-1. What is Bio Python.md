# 1. 바이오파이썬
1-1. 바이오파이썬이란  
 - 생명정보학 프로그래밍을 위한 파이썬 라이브러리
 - 바이오파이썬 활용
   - 파싱작업: 데이터에서 원하는 정보를 추출하는 것
     - 추출 포맷: FASTA, FASTQ, GenBank, KEGC etc.
   - 유전체 서열정보를 문자열 수준에서 작업  
     : 파싱 후의 서열 데이터를 파이썬의 문자열 작업처럼 쉽게 작업 가능
   - 웹 정보 사용 가능  
    : 웹 상에서 접근했던 정보를 프로그래밍 레벨에서 사용 가능
   - 생명정보학에서 사용하는 Tool 사용 가능  
    : BLAST와 같은 서열정리(Sequence Alignment)작업을 통한 분석 가능

1-2. 바이오파이썬을 사용한 활용
 - FASTA 파일 파싱
   - FASTA 파일은 헤더와 서열로 이루어진 파일 형식으로 두 번째 줄부터 120개의 서열이 한줄 씩 표시
   - 파싱 예시
  ```py
  from Bio import SeqIO #바이오파이썬 SeqIO 모듈 호출
  record = SeqIO.read("hemoglobin_subunit_beta.fasta","fasta") 
  #.read method를 사용하여 fasta파일을 변수에 저장
  print(record.id) #fasta 파일의 id 출력(FASTA 파일의 헤더 출력)
  print(len(record.seq)) #fasta 파일의 서열을 len() 함수를 사용하여 길이 출력
  print(record.seq(count("A"))) #fasta 파일 내 염기(아데닌)의 수 출력, 염기는 문자열 형태로 입력
  ```
 - 웹 정보 호출
   - 바이오파이썬을 사용하여 PubMed에서 확인할 수 있는 정보의 호출이 가능
  ```py
  from Bio import Entrez #바이오파이썬 Entrez 모듈 호출
  Entrez.email = "myemail@domain.com" #사용자의 이메일 입력
  handle = Entrez.egquery(term="bioinformatics")
  #bioinformaitcs에 대한 정보를 .egquery method를 사용하여 변수에 저장
  #.egquery method: 데이터베이스에 정보를 요청하는 method
  record = Entrez.read(handle) #변수에 저장된 정보를 .read method를 사용하여 새로운 변수에 저장
  for result in record['eGQueryResult']:
    if result['DbName'] == 'pubmed':
        print(result['Count'])
  >>> 258558
  #bioinformatics로 검색했을 때, PubMed에서 확인할 수 있는 모든 문헌의 수 출력
  ```
 - 웹 정보 호출 및 파일파싱, 서열정보 작업
   - Entrez 모듈을 사용하여 NCBI DB 조회 후 genbank 포맷을 GenBank 모듈로 파싱
  ```py
  from Bio import Entrez #바이오파이썬 Entrez 모듈 호출
  from Bio import GenBank #바이오파이썬 GenBank 모듈 호출
  handle = Entrez.efetch(db="nucleotide", id="KT225476", rettype="gb", retmode="text")
  #변수에 .efetch method를 사용해 원하는 DB, id 입력 후 GenBank 파일 선택
  record = GenBank.read(handle) #.read method를 사용하여 새로운 변수에 저장
  print("Accession: ", record.accession) #저장된 GenBank 파일의 accession id 출력
  print("Organism: ", record.organism) #저장된 GenBank 파일의 organism 출력
  print("Size: ", record.size) #저장된 GenBank 파일의 서열 길이 출력
  ```
  - 생명정보학 Tool 사용
    - Alignment Tool인 BLAST를 NCBIWWW 모듈로 호출하여 결과를 xml 파일로 저장 후 파싱
  ```py
  from Bio.Blast import NCBIWWW #Bio.Blast의 NCBIWWW 모듈 호출
  from Bio import SeqIO #바이오파이썬의 SeqIO 모듈 호출
  record = SeqIO.read("blastSample.fasta", format="fasta")
  #처음 보는 서열이 담긴 fasta파일을 변수에 저장
  result_handle = NCBIWWW.qblast("blastn", "nt", record.format("fasta"))
  # .qblast method를 사용하여 사용할 BLAST, DB 선택 후 진행
  with open("blast_result.xml", "w") as out_handle:
    out_handle.write(result_handle.read())
  result_handle.close()
  #BLAST의 결과를 xml 파일로 저장
  from Bio.Blast import NCBIXML #Bio.Blast의 NCBIXML 모듈 호출
  result_handle = open("blast_result.xml") #Blast 결과가 담긴 xml 파일 실행
  blast_records = NCBIXML.parse(result_handle) #.parse method를 사용해 결과 파싱
  for blast_record in blast_records:
    for alignment in blast_record.alignments:
        for hsp in alignmet.hsps:
            print(alignment.title)
            print(alignment.length)
            print(hsp.expect)
            print(hsp.query)
            print(hsp.match)
            print(hsp.sbjct)
            print("")
  #파싱 결과에서 원하는 정보들 출력