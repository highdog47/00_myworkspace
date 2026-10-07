# CLAUDE.md

이 파일은 Claude Code (claude.ai/code)가 이 저장소의 코드 작업 시 참고할 지침을 제공합니다.

## 프로젝트 개요

**Claude Code 마스터리** - Claude Code와 Claude API를 활용한 개발 기술을 배우기 위한 학습 프로젝트입니다.

### 학습 목표
- Claude Code CLI와 기능들을 심층적으로 이해
- Claude API를 통한 AI 기반 개발 흐름 습득
- 실제 프로젝트에 Claude를 통합하는 방법 학습

## 프로젝트 구조

```
claude-code-mastery/
├── basics/              # Claude Code 기본 사용법
├── advanced/            # 고급 기능 (hooks, MCP, plugins)
├── examples/            # 실제 예제 프로젝트
├── tutorials/           # 단계별 튜토리얼
└── reference/           # API 레퍼런스 및 문서
```

## 학습 경로

### 1단계: 기본 (basics/)
- Claude Code CLI 설치 및 초기화
- 기본 명령어 학습 (`/help`, `/config`, `/fast`)
- 파일 편집 및 코드 생성 기본
- Tool 사용 기초 (Read, Edit, Glob, Grep)

### 2단계: 워크플로우 (examples/)
- 실제 프로젝트 생성
- Git 통합 및 커밋 워크플로우
- PR 리뷰 기능 사용
- 코드 리뷰 및 피드백 루프

### 3단계: 고급 (advanced/)
- Settings 및 Hooks 설정
- 커스텀 Skills 작성
- MCP (Model Context Protocol) 서버 통합
- Plugins 개발 (hooks 모듈)

## 개발 환경 설정

### 필수 요구사항
- Claude Code CLI (최신 버전)
- Node.js 16+ (예제 실행용)
- Git

### 초기 설정
```bash
# Claude Code CLI 설치 (이미 설치된 경우 생략)
npm install -g @anthropic-ai/claude

# 저장소 초기화
cd C:\Users\KJH\00_myworkspace\claude-code-mastery
claude init
```

## 학습 자료 추가 가이드

각 폴더에 새로운 학습 자료를 추가할 때:

1. **코드 예제**: 간단하고 실행 가능한 형태로 제공
2. **설명**: 왜 이 방법을 사용하는지, 어떻게 작동하는지 명확히
3. **실습 과제**: 각 주제마다 적용 연습 문제 포함
4. **참고 자료**: 공식 문서나 추가 학습 자료 링크

## 사용 중 팁

### Claude Code 활용
- `/help` - 사용 가능한 명령어 확인
- `/fast` - 빠른 응답 모드 전환
- `/code-review` - 코드 리뷰 (기본, 울트라 모드)
- `/loop` - 반복 작업 자동화

### Bash 명령어
```bash
# 현재 디렉토리의 파일 구조 확인
tree /F

# 특정 파일 타입 찾기
find . -type f -name "*.js"

# 코드 검색
grep -r "searchterm" .
```

## 학습 진행 추적

각 주제를 완료할 때마다 체크리스트를 업데이트하세요:

- [ ] Claude Code 기본 명령어
- [ ] Tool 사용법 (Read, Edit, Glob, Grep)
- [ ] 첫 번째 프로젝트 생성
- [ ] Git 워크플로우
- [ ] 코드 리뷰 기능
- [ ] Settings 및 Hooks
- [ ] 커스텀 Skills
- [ ] MCP 서버 연동
- [ ] Plugins 개발

## 참고 자료

### 공식 문서
- Claude Code 가이드: https://claude.com/claude-code
- Claude API: https://docs.anthropic.com
- MCP 스펙: https://modelcontextprotocol.io

### 커뮤니티
- GitHub 이슈: https://github.com/anthropics/claude-code/issues
- 피드백: /help 명령어로 제안

---

**마지막 업데이트**: 2026-10-02
