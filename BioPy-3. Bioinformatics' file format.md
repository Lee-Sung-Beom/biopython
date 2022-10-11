# 3. 생명정보학 파일 포맷
3-1. FASTA
 - TEXT 기반 포맷으로 염기서열 / 단백질서열을 나타내기 위해 만든 파일 포맷
 - 파일 형식
   - 헤더(>)와 서열로 이루어짐
   - 한 종류의 FASTA, 여러 종류의 Multi FASTA
   - Multi FASTA의 경우 2개 이상의 헤더 존재
   - 예시
   - <FASTA 파일 삽입>

3-2. FASTQ
 - TEXT 기반 포맷으로 염기서열과 염기서열에 해당하는 퀄리티 점수 포함
 - 총 4줄로 구성되고, 이 4줄이 하나의 리드(Read)로 구성
   - 리드(Read): 시퀀서가 샘플의 서열을 한 번에 읽은 길이
 - 파일 형식
   - 헤더(>): FASTQ의 첫 번째 줄로, 염기서열이 읽힌 시퀀싱에 대한 정보 포함
   - 두 번째 줄: 시퀀서가 읽은 염기서열
   - 세 번째 줄: 구분문자(+)
   - 네 번째 줄: 시퀀서가 읽은 염기서열의 퀄리티 정보
   - 예시
     - <FASTQ 파일 삽입>
 - 염기서열의 퀄리티 정보는 Phred Quality Score 체계를 사용하여 문자로 표시
   - ASCII 코딩 체계에 맞춰 알파벳으로 표시
   - Phred quality score: ASCII - 33의 점수를 다음의 식으로 계산하여 사용 
   - $$Q = -10log_{10}P$$
   - ASCII - 33의 값을 Q에 대입하여 P의 값을 구한 후 P - 1로 계산한 값이 염기를 맞게 읽을 확률
     - (P - 1) x 100(%)의 확률로 염기를 정확히 읽었다는 의미

3-3. SAM/BAM
 - SAM(Sequence Alignment Map): TEXT 기반 포맷으로 리드의 정렬된 데이터 포함
 - BAM(Binary Alignment Map): TEST 기반 포맷으로 2진 형식의 파일로 압축
   - 압축률이 SAM보다 뛰어남
   - SAM보다 파일 크기가 매우 작음
 - 파일 형식
   - 헤더(@): 정렬한 시퀀스에 대한 정보, 정렬에 사용한 샘플 정보, 사용 툴에 대한 정보 포함
   - 정렬 부분
      |열|예시|설명|
      |:---:|:---:|:---:|
      |리드 ID|SRR00982.5|리드의 ID 표시|
      |SAM Flag|67|2진수 비트 플래그|
      |리드가 붙은 염색체|chrX|염색체 번호|
      |정렬 위치|26266805|위치 정보|
      |맵핑 퀄리티 점수|60|맵핑이 얼마나 잘 되었는지|
      |시가 문자열|44M|CIGAR 문자열|
      |짝리드가 붙은 염색체|=|Pair로 시퀀싱된 짝리드의 염색체 위치|
      |짝리드의 정렬 위치|26268926|짝리드의 위치 정보|
      |인서트 크기|2122|포워드 리드와 리버스 리드의 간격|
      |리드 염기서열|TACAAT|염기서열 표현|
 - SAM flag: 리드가 정렬된 상태를 SAM flag 조합으로 설명
    - 10진수의 정수를 bin( ) 함수를 사용하여 2진수로 표시
    - 2진수로 변환 후 1이 있는 자리수의 리드에 짝이 있음
    - 짝리드에서 첫 번째 리드를 의미(first in pair)
    ```py
    bin(67)
    >>> '0b1000011'
    # <- 1 방향으로 순서를 확인
    ```
 - CIGAR 문자열: 정해진 문자를 통해 정렬에서 어느 위치에서 변경, 일치 등에 대한 정보 확인 가능
    |문자열|설명|
    |---|---|
    |M|정렬된 리드가 일치 (Match)|
    |I|기준 서열에 정렬 시 염기 추가, 기준 서열에 없고 리드에만 존재 (Insertion)|
    |D|기준 서열에 정렬 시 염기 제거, 기준 서열에 있고 리드에 없음 (Deletion)|
    |N|기준 서열과 비교 시 염기를 건너뜀 (Skipped region)|
    |S|리드의 염기가 잘렸지만 SAM/BAM 파일에 존재 (Soft clip)|
    |H|리드의 염기가 잘려 SAM/BAM 파일에 남지 않음 (Hard clip)|
    |P|기준 서열에는 없지만 리드에 추가된 패딩 서열 (Padding)|
    - CIGAR 문자열 예시
      |6M| | | | | | | | | | |
      |---|---|---|---|---|---|---|---|---|---|---|
      |기준 서열|A|C|A|G|T|A|A|T|C|G|
      |리드| | |A|G|T|A|A|T| | |

      |2M1I1M1D2M| | | | | | | | | | | | | |
      |---|---|---|---|---|---|---|---|---|---|---|---|---|---|
      |기준 서열|A|C|A|G|*|T|A|A|T|C|G|
      |리드| | |A|G|G|T|*|A|T| | |
      |설명| | |M|M|I| | |D| | | |

3-4. BED
 - Browser Extensible Data로 유전체를 구간별로 나누어 구간의 특징을 주석으로 표기할 수 있는 파일 형식
 - 각 항목들은 탭으로 구분된 텍스트 파일
 - 파일 형식
    |필수열|설명|예시|
    |---|---|---|
    |chrom|염색체|chr5|
    |chromStart|구간의 시작 지점|55220083|
    |chromEnd|구간의 종료 지점|55220357|
 - BED 파일 예시
    |chrom|chromStart|chromEnd|구간 명|구간 길이|
    |---|---|---|---|---|
    |chr1|100|200|Gene 1|100bp|
    |chr3|300|450|Gene 2|150bp|

3-5. VCF
 - Variant Calling Format, 변이를 표기하기 위한 파일
 - 메타데이터와 내용 부분
   - 내용 부분에는 8개의 필수 열과 샘플에 따라 열 추가
 - 파일 형식
   - 메타데이터(#): ##으로 시작하며, 키=값 관계로 표현
      |값|설명|
      |---|---|
      |##fileformat=|VCF 파일의 버전 기준|
      |##FORMAT=<ID=AD, ..., Description>|쉼표로 구분되어 ref, alt를 순서에 맞게 표시|
      |##FORMAT=<ID=DP, ..., Description>|위치의 depth를 표시|
      |##FILTER=<SNP_filter, Description>|Description에 기재된 필터 조건에 해당하면 SNP_filter, 맞지 않으면 PASS|
   - 필수 열: 유전체 및 변이에 대한 정보 **(다음의 예시는 대표적으로 사용되는 열)**
      |값|설명|예시|
      |---|---|---|
      |CHROM|염색체 번호|chr19|
      |POS|위치|45412079|
      |ID|세미콜론으로 구분되며, 주로 rsID 기입|rs7412|
      |REF|기준 서열의 염기|C|
      |ALT|변이로 나타난 염기|T|
      |QAUL|Phred-scaled quality로, ALT 서열이 틀릴 확률|136.03|
      |FILTER|메타데이터의 FLITER 조건에 따라 표기|조건 부합 = SNP_filter / 조건 미부합 = PASS|
      |INFO|추가 정보 기재, annotation 툴을 사용하여 텍스트 추가|AC=2;AF=0.54|
      |FORMAT|메타데이터의 FORMAT 부분의 정보를 콜론 기준으로 표기|GT:AD:DP*|
      |Sample|열에 맞게 샘플 정보 표기|0/1:30, 35:65
      
      **GT = Genotype / AD = Allelic Depths / DP = Approximate read depth**

3-6. GeneBank
 - NCBI에서 제공하는 포맷으로 염기 서열과 CDS 별로 번역된 아미노산 서열, 종의 정보, 관련 논문 저자, 제목, pubmedID 등 메타정보 포함
 - 파일 형식
   - 메타데이터
      |항목|설명|예시|
      |---|---|---|
      |LOCUS|Accession ID, 길이, 분자 종류<br>GenBank Division 정보, 최종 수정 날짜|KT2254676 29809bp RNA linear<br>VRL 22-AUG-2017|
      |DEFINITION|서열에 대한 간략한 설명|Middle East respiratory syndrome<br>corona virus isolate|
      |ACCESSION|서열의 독자적인 ID|KT225476|
      |VERSION|서열에 변화가 생기면 버전 업데이트|KT225476.2|
      |KEYWORDS|서열을 설명하는 키워드로 없다면 . 으로 표기||
      |SOURCE|서열의 근원에 대한 정보|MERS-CoV|
      |REFERENCE|서열에 관한 논문 정보 포함||
      |COMMENT|기타 설명 포함||
    - 서열 특징: FEATURE 부분에 전체 서열 구간 정보(source)와 CDS(Coding Sequence) 정보 나열
    - 서열: ORIGIN 부분으로 60개 염기 서열을 한 줄로 10개 단위씩 구분

3-7. XML
 - 구조화된 데이터를 표현하고 정보를 교환할 때 범용적으로 사용하는 마크업 언어로 태그를 이용하여 데이터 구조 표시
 - 파일 형식: <태그>내용</태그>
  ```html
  <!DOCTYPE HTML>
  <html>
    <head>
      <title>Hello World</title>
    </head>
    <body>
      <p>Hello World</p>
    </body>
  </html>
  ```

3-8. JSON
 - 구조화된 데이터를 표현하고 정보를 교환할 때 범용적으로 사용하는 파일 포맷
 - XML과의 기능, 목적의 중복은 많이 되지만, 가독성이 XML보다 좋아 대안으로 사용
 - 파일 형식
   - 키-값 (Key-Value)로 구성
  ```json
  {"name":"rs6512", "var_clas":"SNP", "ancestral_allele":"G", "minor_allele":"A", "MAF":0.238675}
  ```***