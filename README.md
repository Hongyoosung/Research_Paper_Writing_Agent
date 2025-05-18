# Research_Paper_Writing_Agent

IEEE 형식의 LaTeX 출력으로 맞춤형 학술 논문을 생성하는 도구

<br>

#

### 📋 개요
연구 논문 작성 에이전트는 Google Colab을 기반으로 고품질 학술 논문 작성을 간소화하는 도구입니다. 주요 기능은 다음과 같습니다:

맞춤형 연구 주제: 전용 셀에서 연구 주제, 키워드, 제목 프롬프트를 설정할 수 있습니다.
조정 가능한 논문 길이: 각 섹션의 단어 수를 설정 셀에서 자유롭게 조정 가능합니다.
강력한 로깅 및 디버깅: 상세한 로그와 품질 검사를 통해 신뢰할 수 있는 결과를 보장합니다.
<br>

#

### ✨ 주요 기능

자동 논문 생성: 적절한 구조와 인용을 포함한 완전한 학술 논문을 생성합니다.
문헌 통합: ArXiv에서 관련 논문을 가져와 Chroma DB에 저장 후 검색 증강 생성(RAG)을 수행합니다.
품질 보증: 섹션의 길이, 관련성, 인용, 가독성을 기준으로 품질을 평가합니다.
LaTeX 출력: BibTeX 인용과 함께 IEEE 형식의 PDF 논문을 생성합니다.
유연한 설정:
셀 13에서 연구 주제와 키워드를 수정 가능.
셀 14에서 섹션 길이(예: 초록, 서론) 조정 가능.


결과 패키징: PDF, LaTeX 파일, BibTeX 참조, 로그, 섹션 JSON을 포함한 zip 파일을 제공합니다.

#

### 🛠️ 요구 사항

환경: 인터넷 연결이 가능한 Google Colab.
종속성 (셀 1에서 설치):
Python 라이브러리: openai, chromadb, arxiv, requests, torch, sentence-transformers, langdetect, scikit-learn, rouge.
시스템 패키지: texlive, texlive-latex-extra, texlive-fonts-recommended, texlive-science, texlive-publishers.


API 키: Colab의 userdata에 OPENAI로 저장된 OpenAI API 키.
Google Drive: 출력 저장을 위해 마운트 (셀 1에서 설정).

#

### 🚀 설정 방법

#### Colab에서 열기:

`research_paper_agent.ipynb`를 Google Colab에 업로드.
Google Drive 계정이 연결되어 있는지 확인.

<br>

#### API 키 설정:

Colab 사이드바의 "Secrets" 탭(자물쇠 아이콘)으로 이동.
OpenAI API 키를 OPENAI라는 이름으로 새로운 비밀로 추가.

<br>

#### 노트북 실행:

모든 셀을 순차적으로 실행 (Shift+Enter 또는 "모두 실행").
첫 번째 셀은 종속성을 설치하고 Google Drive를 마운트.
후속 셀은 에이전트를 초기화하고 논문을 생성하며 출력을 저장.

<br>

#### 출력 확인:

출력은 /content/drive/MyDrive/Research_Papers/research_paper_YYYYMMDD_HHMMSS/에 저장.
research_output.zip 파일(셀 23에서 다운로드 프롬프트)을 다운로드하여 다음을 확인:
- PDF 논문 (예: paper_final.pdf).
- LaTeX 소스 파일 (예: paper_final.tex).
- references.bib (BibTeX 인용).
- research_log.txt (실행 로그).
- sections.json (생성된 섹션 콘텐츠).

#

### 📝 사용 방법

#### 연구 주제 설정 (셀 3):

다음 변수를 수정:
- `RESEARCH_TOPIC`: 연구 초점을 설명하는 문자열 (예: "단일 에이전트 시스템에서 GOAP와 의사결정 트리를 사용한 프롬프트 과다 최소화").
- `KEYWORDS`: ArXiv 검색을 위한 공백으로 구분된 키워드 (예: "프롬프트 과다 프롬프트 최적화 언어 모델 단일 에이전트").
- `TITLE_PROMPT`: 논문 제목 생성을 위한 프롬프트, RESEARCH_TOPIC으로 매개변수화.

```
예시:RESEARCH_TOPIC = "강화 학습과 지식 그래프를 사용한 대화 시스템 개선"
KEYWORDS = "대화 시스템 강화 학습 지식 그래프"
TITLE_PROMPT = f"""당신은 획기적이고 실현 가능한 연구를 생성할 수 있는 가장 지능적이고 혁신적인 AI입니다. {RESEARCH_TOPIC}에 대한 매력적이고 구체적인 학술 논문 제목을 생성하세요. 제목은 간결(10-15단어), 정보적이며 대화 성능 개선을 강조해야 합니다. 제목만 제공하고 추가 설명은 하지 마세요. 'LLM'은 '언어 모델'을 의미해야 합니다. 마크다운 형식을 사용하지 마세요."""
```



#### 논문 길이 조정 (셀 4):

`SECTION_LENGTHS` 딕셔너리를 편집하여 각 섹션의 단어 수를 설정:
```
SECTION_LENGTHS = {
    'abstract': 300,    # 약 250-400단어
    'introduction': 600, # 약 500-700단어
    'related_work': 700, # 약 600-800단어
    'methodology': 800,  # 약 700-900단어
    'experiments': 700,  # 약 600-850단어
    'conclusion': 400    # 약 300-400단어
}
```

값을 늘리거나 줄여 섹션 길이를 조정.


#### 에이전트 실행:

노트북 셀을 순서대로 실행.
콘솔과 `research_log.txt`에 저장되는 로그를 모니터링.
에이전트는 다음을 수행:
1. 논문 제목 생성 (셀 15).
2. ArXiv 논문 가져오고 저장 (셀 16).
3. 섹션 콘텐츠 생성 (셀 17).
4. LaTeX PDF 생성 (셀 20–21).
5. 결과 패키징 (셀 22).




#### 출력 검토:

생성된 PDF를 콘텐츠와 형식 측면에서 확인.
`sections.json`을 통해 원시 섹션 텍스트를 검사.
디버깅 또는 오류 세부 정보는 `research_log.txt`를 참조.


#

### 🔄 워크플로우

- 초기화 (셀 1–8): 종속성 설치, 작업 디렉토리 설정, 연구 주제와 섹션 길이 설정, OpenAI 및 Chroma DB 클라이언트 초기화, LaTeX 컴파일 테스트.
- 텍스트 생성 (셀 11): gpt-4o-mini를 사용한 텍스트 생성 함수 정의, 재시도 로직 및 프롬프트 정리 포함.
- 문헌 수집 (셀 12): `KEYWORDS` 기반으로 ArXiv 논문을 가져와 Chroma DB에 저장.
- 논문 생성 (셀 15–17): 제목 및 섹션 생성, RAG를 활용한 관련 연구 통합, 품질 점수 매기기.
- LaTeX 컴파일 (셀 18–21): 콘텐츠를 LaTeX로 변환, PDF 컴파일, 인용 처리.
- 출력 패키징 (셀 22): 모든 출력을 다운로드용 zip 파일로 압축.
- 요약 및 디버깅 (셀 23–24): 품질 점수, 실행 시간, 파일 세부 정보 요약 제공.

#

📌 참고 사항

- 성능: 실행 시간은 API 응답 시간과 ArXiv 가용성에 따라 5–15분 소요.
- 오류 처리: 강력한 로깅 및 재시도 메커니즘 포함. 오류(예: API 실패, LaTeX 컴파일 문제)는 research_log.txt에서 확인.
- 사용자 지정: 주제와 길이 외에도 고급 사용자는 셀 17의 프롬프트를 수정하여 콘텐츠를 맞춤화 가능.

#### 제한 사항:
- Colab secret access key "OPENAI"에 api키 설정 필요.
- API 호출 및 ArXiv 접근을 위해 안정적인 인터넷 연결 필요.
- 생성된 콘텐츠 품질은 gpt-4o-mini 성능에 의존.
- 특수 문자가 제대로 이스케이프되지 않으면 LaTeX 컴파일이 실패할 수 있음.


#### 문제 해결:
- OpenAI API 실패 시, userdata의 API 키 확인.
- LaTeX 오류의 경우, 출력 디렉토리의 `compile_step_*.log` 파일 확인.
- ArXiv 가져오기 실패 시, `KEYWORDS` 조정 또는 셀 16의 `num_papers` 감소.


#

### 📄 출력 예시
기본 주제("단일 에이전트 시스템에서 GOAP와 의사결정 트리를 사용한 프롬프트 과다 최소화")의 경우, 에이전트는 다음을 생성:

- 제목 예시: "GOAP와 의사결정 트리를 사용한 단일 에이전트 LLM의 프롬프트 효율성 최적화".
- 섹션: ArXiv 논문(예: [paper0])을 인용하며, 각 섹션은 300–800단어로 구성.
- 최종 출력: IEEE 형식의 PDF (paper_final.pdf).
- 다운로드: 모든 아티팩트를 포함한 research_output.zip 파일.

#
 
### 📜 License
This project is licensed under the MIT License. See the LICENSE file for details (not included here).


