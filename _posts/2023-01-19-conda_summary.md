---
layout: single
title:  "[요약] Anaconda 각종 명령어"
categories:
    - Anaconda
tag: [Summary, Conda, Anaconda, Environment, Jupyter Notebook]

date: 2023-01-19
last_modified_at: 2023-01-19
---

# Anaconda 및 Miniconda 사용법
## Conda 버전 확인
- `conda -V`
- `conda --version`

## 가상환경 생성
```
$ conda create -n test_env python=3.8
```
Python 3.5 버전의 'test_env'라는 이름으로 `env`를 생성

## env list 보기
```
$ conda env list
```
or
```
$ conda info --envs
```

## env 활성화
```
$ conda activate test_env
```

## env 비활성화
```
$ conda deactivate
```

## env 삭제
```
$ conda env remove -n test_env
```

## env 복사
```
$ conda create -n test_env_clone --clone test_env
```

## env 내보내기
가상환경 실행 후 -> yaml 파일 생성
```
$ conda env export > test_env.yaml
```

## env 가져오기
yaml 파일이 있는 경로에서 실행
```
$ conda env create -f test_env.yaml
```

## package install
```
$ conda install [~~]
```
채널을 이용하면
```
$ conda install -c conda-forge [~~]
```

## conda-forge 채널 추가하기
```
$ conda config --add channels conda-forge
```


## 등록된 채널 확인하기
```
$ conda config --show channels
```

## 가장 낮은 우선순위의 채널 추가
```
$ conda config --append channels {채널명}
```

## 채널 삭제
```
$ conda config --remove channels {채널명}
```

# Jupyter Notebook
## Jupyter Notebook install
```
$ conda insatll jupyter
```

## ipykernel (가상환경 별로 jupyter kernel에 연결시켜줘야함)
```
$ conda install ipykernel
```
```
$ python -m ipykernel install --user --name=가상환경이름
```

## kernel 목록 보기
```
$ jupyter kernelspec list
```

## 제거하기
```
$ jupyter kernelspec uninstall 가상환경이름
```
