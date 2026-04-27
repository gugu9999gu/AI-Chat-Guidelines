# AI Project Harness Instructions

> AI coding agents often write code quickly, but they can easily lose project context, skip documentation, forget verification, or expose sensitive information.  
> This repository provides a reusable instruction set that turns an AI coding assistant into a more disciplined **lead developer + project manager** for long-running software projects.

---

## 🇰🇷 한국어

### 개요

이 저장소는 AI 개발 도구에 적용할 수 있는 **채팅 지침**입니다.

ChatGPT, Claude Code, Cursor, Windsurf, Codex, GitHub Copilot Chat 등 AI 코딩 도구에 이 지침을 적용하면 단순히 코드를 생성하는 수준을 넘어, 프로젝트의 문맥 유지, 문서화, 변경 이력 관리, 보안, 검증, 핸드오프까지 일관되게 관리하도록 유도할 수 있습니다.

이 지침의 핵심 목적은 다음과 같습니다.

- AI가 프로젝트의 현재 상태를 잃지 않도록 한다.
- 작업 전후로 필요한 문서를 읽고 업데이트하게 한다.
- 변경 내역을 `history.md`에 남기게 한다.
- 프로젝트 의도와 진행 상황을 `guideline.md`에 유지하게 한다.
- UI/UX 변경 사항을 `designguide.md`에 기록하게 한다.
- 민감정보 하드코딩을 방지한다.
- 빌드, 타입 체크, 린트 등 검증 절차를 강제한다.
- 다른 AI 에이전트나 개발자가 이어받기 쉽게 핸드오프 정보를 남기게 한다.

---

### 적용하면 무엇이 좋아지나요?

#### 1. 프로젝트 맥락 유지

AI는 긴 프로젝트를 진행하다 보면 이전 결정, 파일 역할, 현재 진행률, 다음 작업을 놓치기 쉽습니다.  
이 지침은 `guideline.md`를 프로젝트의 중심 문서로 사용하게 하여 AI가 작업 전 항상 현재 상태를 확인하도록 만듭니다.

결과적으로 다음 문제가 줄어듭니다.

- 이미 만든 기능을 다시 구현하는 문제
- 기존 구조와 다른 방식으로 코드를 추가하는 문제
- 프로젝트 방향과 맞지 않는 코드가 섞이는 문제
- 다음 작업자가 어디서부터 이어가야 할지 모르는 문제

---

#### 2. 작업 이력 관리

모든 작업은 `history.md`에 기록됩니다.

예를 들어 다음과 같은 정보가 남습니다.

- 어떤 파일을 수정했는지
- 무엇을 구현했는지
- 어떤 파일을 삭제했는지
- 왜 삭제했는지
- 롤백이나 검토가 필요한 작업은 무엇인지

이 방식은 AI가 만든 변경사항을 추적하기 쉽게 만들고, 문제가 생겼을 때 원인을 빠르게 찾는 데 도움이 됩니다.

---

#### 3. AI 에이전트 간 핸드오프 개선

Claude Code에서 작업하다가 Cursor로 옮기거나, ChatGPT에서 설계한 내용을 Codex나 Copilot으로 이어가는 경우가 많습니다.

이 지침은 작업 종료 시 다음 정보를 남기도록 요구합니다.

- 마지막으로 수정한 파일 목록
- 현재 블로커 또는 미해결 이슈
- 다음 단계로 해야 할 구체적인 작업 1~3개

덕분에 다른 AI나 사람이 이어받을 때 프로젝트를 다시 처음부터 파악하는 시간을 줄일 수 있습니다.

---

#### 4. 보안 사고 예방

이 지침은 API 키, DB 비밀번호, 시크릿 토큰을 코드에 하드코딩하지 못하도록 명확히 제한합니다.

또한 `.env`, `.env.local`, `.env.production`, `node_modules/`, `.next/`, `dist/`, `build/` 등 Git에 올라가면 안 되는 파일을 `.gitignore`에 포함하도록 요구합니다.

기대 효과는 다음과 같습니다.

- API 키 유출 방지
- 환경변수 관리 습관 강화
- 배포용 코드와 로컬 비밀값 분리
- GitHub 공개 저장소에서 민감정보 노출 가능성 감소

---

#### 5. UTF-8 및 한글 인코딩 안정성 강화

한국어 프로젝트에서는 Windows, PowerShell, Node.js, Docker, PostgreSQL, Python 환경에서 인코딩 문제가 자주 발생합니다.

이 지침은 환경별 UTF-8 규칙을 명시합니다.

예:

- Windows `.bat`: `chcp 65001 >nul`
- PowerShell: `[Console]::OutputEncoding = [System.Text.Encoding]::UTF8`
- Docker: `ENV LANG=C.UTF-8`
- PostgreSQL: `ENCODING 'UTF8'`

이를 통해 한글 깨짐, 터미널 출력 오류, 환경변수 문자 깨짐 문제를 줄일 수 있습니다.

---

#### 6. 검증 절차 강제

AI가 코드를 작성한 뒤 검증 없이 끝내는 경우가 많습니다.  
이 지침은 작업 후 반드시 검증을 수행하도록 요구합니다.

대표 검증 항목은 다음과 같습니다.

- `npm run build`
- `npx tsc --noEmit`
- `npm run lint`
- 변경 파일을 import하는 다른 파일 확인
- 새 환경변수가 `.env.example`에 반영되었는지 확인

즉, “코드를 작성했다”에서 끝나는 것이 아니라 “동작 가능한 상태인지 확인했다”까지 가도록 유도합니다.

---

#### 7. 불필요한 코드 정리 기준 제공

AI는 기존 코드를 함부로 지우거나, 반대로 필요 없는 코드를 계속 남기는 경우가 있습니다.

이 지침은 사용하지 않는 파일이나 코드를 삭제할 수 있게 하되, 삭제 전 다음을 반드시 확인하게 합니다.

1. 삭제 대상과 이유를 사용자에게 알림
2. `history.md`에 삭제 경로와 사유 기록
3. 다른 파일에서 참조하고 있지 않은지 확인

이 방식은 코드베이스를 깔끔하게 유지하면서도 추적 가능한 삭제 이력을 남깁니다.

---

### 권장 파일 구조

```text
project-root/
├─ guideline.md      # 프로젝트의 현재 상태, 의도, 진행률, 파일 역할, 핸드오프 정보
├─ history.md        # 모든 작업 기록, 삭제 기록, 롤백 기록
├─ designguide.md    # 디자인 시스템, UI/UX 규칙, 레이아웃 변경 이력
├─ README.md         # 프로젝트 소개, 설치, 실행, 배포, 환경변수, 보안 안내
├─ .env.example      # 환경변수 예시, 실제 값 제외
├─ .gitignore
└─ src/
```

---

### 권장 사용 방법

#### ChatGPT / Claude / Cursor / Windsurf 등에 적용

1. 이 저장소의 지침 파일을 복사합니다.
2. 사용하는 AI 도구의 Custom Instructions, Project Rules, System Prompt, Memory, Rules 파일 등에 붙여넣습니다.
3. 프로젝트 루트에 다음 문서를 생성합니다.
   - `guideline.md`
   - `history.md`
   - `designguide.md`
   - `README.md`
4. AI에게 작업을 요청할 때 다음처럼 말합니다.

```text
이 프로젝트는 하네스 지침을 따른다.
작업 전 guideline.md와 history.md를 읽고,
작업 후 변경사항을 문서에 반영해줘.
```

---

### 적합한 프로젝트

이 지침은 특히 다음 프로젝트에 적합합니다.

- 장기적으로 유지보수할 웹앱
- Next.js, React, Node.js, Python 기반 프로젝트
- 여러 AI 도구를 번갈아 사용하는 프로젝트
- 팀원 또는 외주 개발자에게 넘겨야 하는 프로젝트
- 문서화와 히스토리 관리가 중요한 프로젝트
- 한글 인코딩 안정성이 중요한 프로젝트
- 공개 GitHub 저장소로 관리할 프로젝트

---

### 주의사항

이 지침은 AI의 실수를 완전히 없애는 도구가 아닙니다.  
다만 AI가 작업할 때 반드시 확인해야 할 기준과 절차를 제공하여 다음 문제를 줄이는 데 목적이 있습니다.

- 맥락 손실
- 문서 누락
- 검증 생략
- 보안 실수
- 인코딩 문제
- 핸드오프 실패
- 무분별한 파일 삭제

중요한 배포, 결제, 인증, 데이터베이스 마이그레이션, 보안 관련 작업은 반드시 사람이 최종 검토해야 합니다.

---

## 🇺🇸 English

### Overview

This repository provides a reusable **Project Harness Instruction Set** for AI coding tools.

When applied to ChatGPT, Claude Code, Cursor, Windsurf, Codex, GitHub Copilot Chat, or similar AI development assistants, these instructions encourage the AI to act not only as a code generator, but also as a disciplined **lead developer and project manager**.

The main goal is to help AI agents maintain project context, update documentation, manage work history, follow security rules, verify changes, and leave clear handoff information for the next developer or AI agent.

---

### What improves when you apply this?

#### 1. Better project context retention

AI agents can lose track of previous decisions, file roles, current progress, and next steps during long-running projects.  
This instruction set makes `guideline.md` the central project brain, forcing the AI to read and update the project state before and after work.

This helps reduce:

- Re-implementing already completed features
- Adding code that does not match the existing architecture
- Losing sight of the original project intent
- Making it hard for the next developer or AI agent to continue

---

#### 2. Clear work history

Every implementation, modification, deletion, rollback, and review item should be recorded in `history.md`.

This makes it easier to track:

- Which files were changed
- What was implemented
- What was deleted
- Why something was deleted
- Which tasks still require review or testing

This is especially useful when debugging regressions or reviewing AI-generated changes.

---

#### 3. Improved handoff between AI agents

It is common to move work between Claude Code, Cursor, ChatGPT, Codex, Copilot, Windsurf, or human developers.

This instruction set requires the current agent to leave a handoff checkpoint containing:

- Recently modified files
- Current blockers or unresolved issues
- 1 to 3 concrete next steps

This reduces the time needed for the next agent or developer to understand the project.

---

#### 4. Better security hygiene

The instructions explicitly prohibit hardcoding API keys, database passwords, and secret tokens in source code.

They also require sensitive files and generated outputs to be included in `.gitignore`, such as:

- `.env`
- `.env.local`
- `.env.production`
- `node_modules/`
- `.next/`
- `dist/`
- `build/`

This helps reduce the risk of leaking secrets in public GitHub repositories.

---

#### 5. UTF-8 and Korean encoding stability

Korean-language projects often face encoding issues across Windows, PowerShell, Node.js, Docker, PostgreSQL, and Python environments.

This instruction set defines environment-specific UTF-8 rules such as:

- Windows `.bat`: `chcp 65001 >nul`
- PowerShell: `[Console]::OutputEncoding = [System.Text.Encoding]::UTF8`
- Docker: `ENV LANG=C.UTF-8`
- PostgreSQL: `ENCODING 'UTF8'`

This helps prevent broken Korean text, terminal output issues, and environment variable encoding problems.

---

#### 6. Mandatory verification workflow

AI tools often stop after generating code without verifying whether it actually works.  
This instruction set requires the AI to verify changes after implementation.

Recommended verification includes:

- `npm run build`
- `npx tsc --noEmit`
- `npm run lint`
- Checking files that import modified modules
- Ensuring new environment variables are reflected in `.env.example`

The goal is to move from “code was written” to “the change was checked.”

---

#### 7. Safer cleanup and deletion rules

AI agents may delete files too aggressively or leave unused code behind forever.

This instruction set allows unused code to be removed, but only after:

1. Informing the user about the deletion target and reason
2. Recording the deleted path and reason in `history.md`
3. Checking that no other files reference the deleted module

This keeps the codebase clean while preserving traceability.

---

### Recommended Project Structure

```text
project-root/
├─ guideline.md      # Project state, intent, progress, file roles, handoff information
├─ history.md        # Work log, deletion records, rollback records
├─ designguide.md    # Design system, UI/UX rules, layout change history
├─ README.md         # Project overview, setup, deployment, environment variables, security notes
├─ .env.example      # Example environment variables without real values
├─ .gitignore
└─ src/
```

---

### Recommended Usage

#### Apply to ChatGPT / Claude / Cursor / Windsurf / Copilot

1. Copy the instruction file from this repository.
2. Paste it into your AI tool’s Custom Instructions, Project Rules, System Prompt, Memory, or Rules file.
3. Create the following files in your project root:
   - `guideline.md`
   - `history.md`
   - `designguide.md`
   - `README.md`
4. When asking the AI to work on your project, use a prompt like:

```text
This project follows the harness instructions.
Before working, read guideline.md and history.md.
After completing the task, update the relevant documentation.
```

---

### Best-Fit Projects

This instruction set is especially useful for:

- Long-term web application projects
- Next.js, React, Node.js, or Python projects
- Projects using multiple AI coding tools
- Projects that will be handed off to teammates or contractors
- Projects where documentation and work history matter
- Korean-language projects that require encoding stability
- Public GitHub repositories

---

### Important Notes

This instruction set does not eliminate AI mistakes completely.  
Its purpose is to provide a structured workflow that reduces common AI development problems, such as:

- Losing project context
- Skipping documentation
- Skipping verification
- Mishandling secrets
- Breaking Korean encoding
- Failing handoff
- Deleting files without traceability

Human review is still required for important production changes, especially around deployment, payment, authentication, database migrations, and security-sensitive code.

---

## Suggested Repository Files

You can organize this repository like this:

```text
ai-project-harness-instructions/
├─ README.md
├─ instructions.ko.md
├─ instructions.en.md
├─ templates/
│  ├─ guideline.md
│  ├─ history.md
│  └─ designguide.md
└─ LICENSE
```

---

## License

MIT License is recommended if you want others to freely use, modify, and share this instruction set.

---

## Keywords

AI coding assistant, project harness, AI development workflow, Claude Code, Cursor rules, ChatGPT instructions, GitHub Copilot, Codex, documentation workflow, project handoff, UTF-8, Korean encoding, secure AI coding
