# 국회 회의록 요약 Baseline
본 리포지토리는 '2024년 국립국어원 인공지능의 한국어 능력 평가' 상시 과제 중 '국회 회의록 요약'에 대한 베이스라인 모델의 학습과 평가를 재현하기 위한 코드를 포함하고 있습니다.  

학습, 추론의 실행 방법(How to Run)은 아래에서 확인하실 수 있습니다.   

|Model|ROUGE-1|
|:---|---|
|MLP-KTLim/llama-3-Korean-Bllossom-8B (without SFT)|0.26806|
|MLP-KTLim/llama-3-Korean-Bllossom-8B (with SFT)|0.37988|

## 리포지토리 구조 (Repository Structure)
```
# 학습에 필요한 리소스들을 보관하는 디렉토리
resource
└── data

# 실행 가능한 python 스크립트를 보관하는 디렉토리
run
├── test.py
└── train.py

# 학습에 사용될 커스텀 함수들을 보관하는 디렉토리
src
├── data.py     # Custom Dataset
└── utils.py
```

## 데이터 (Data)
```
{
    "id": "nikluge-2024-국회 회의록 안건별 요약-train-000001",
    "input": {
        "speaker": [
            {
                "id": "김상희",
                "occupation": "소위원장",
                "original_id": "김상희"
            },
            {
                "id": "김귀순",
                "occupation": "수석전문위원",
                "original_id": "김귀순"
            },
            ...
        ],
        "conversation": [
            {
                "id": "SBRW2100000001.1.1.1",
                "speaker": "김상희",
                "utterance": "회의를 시작하도록 하겠습니다."
            },
            {
                "id": "SBRW2100000001.1.1.2",
                "speaker": "김상희",
                "utterance": "제284회 국회(정기회) 여성위원회 제1차 예산결산기금심사소위원회를 개의하겠습니다."
            },
            {
                "id": "SBRW2100000001.1.1.3",
                "speaker": "김상희",
                "utterance": "바쁘신 중에도 이렇게 회의에 참석해 주신 여러분들께 감사를 드립니다."
            },
            {
                "id": "SBRW2100000001.1.1.4",
                "speaker": "김상희",
                "utterance": "그러면 바로 의사일정에 들어가도록 하겠습니다."
            },
            {
                "id": "SBRW2100000001.1.1.5",
                "speaker": "김상희",
                "utterance": "의사일정 제1항 2008회계연도 여성부 소관 세입세출결산, 의사일정 제2항 2008회계연도 여성발전기금 결산, 이상 2건을 일괄하여 상정합니다."
            },
            {
                "id": "SBRW2100000001.1.1.6",
                "speaker": "김상희",
                "utterance": "오늘 소위원회 회의 진행은 지난번 전체회의 대체토론 시에 위원님들께서 지적하신 사항과 검토보고를 토대로 수석전문위원이 정리한 결산시정요구사항을 중심으로 해서 수석전문위원으로부터 설명을 듣고 위원님들의 의견과 정부 측 답변을 들은 다음 사안별로 정리하는 방법으로 회의를 진행하고자 합니다."
            },
            ...
        ],
        "issue": {
            "topic": "1. 2008회계연도 세입세출결산 > 가. 여성부 소관",
            "keyword": "2008회계연도 여성부 소관 세입세출결산",
            "sentence_id": "SBRW2100000001.1.1.5",
            "begin": 9,
            "end": 31
        }
    },
    "output": "본 회의에서 여성부의 2008회계연도 세입세출결산에 관해 세입 징수 결정된 금액의 정산 잔액을 조속히 반납 조치가 필요하다고 했고, 여성부는 다 납부가 되었고 수납률 제고를 하겠다고 답변하여 해당 사안은 넘어갔다. 그리고 이·전용 과다 예산 10% 절감 부당에 관해서 위원들은 \"시정\"으로 의견을 모았지만, 다시 결정하기로 하고 넘어갔고, 이·전용액으로 여성부가 청사 임차료를 충당한 문제에 대해서는 여성부의 해명을 듣고 불가피한 전용이었다고 보고 \"주의\"를 주기로 했다."
}
```

## 실행 방법 (How to Run)
### 학습 (Train)
```
CUDA_VISIBLE_DEVICES=1,3 python -m run.train \
    --model_id MLP-KTLim/llama-3-Korean-Bllossom-8B \
    --batch_size 1 \
    --gradient_accumulation_steps 64 \
    --epoch 5 \
    --lr 2e-5 \
    --warmup_steps 20
```

### 추론 (Inference)
```
python -m run.test \
    --output result.json \
    --model_id MLP-KTLim/llama-3-Korean-Bllossom-8B \
    --device cuda:0
```

## Reference

huggingface/transformers (https://github.com/huggingface/transformers)  
Bllossome (Teddysum) (https://huggingface.co/MLP-KTLim/llama-3-Korean-Bllossom-8B)  
국립국어원 인공지능 (AI)말평 (https://kli.korean.go.kr/benchmark)  
