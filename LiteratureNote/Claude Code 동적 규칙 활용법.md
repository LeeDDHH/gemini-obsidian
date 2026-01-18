# Claude Code 동적 규칙 활용법

**Original URL**: https://zenn.dev/tmasuyama1114/articles/claude_code_dynamic_rules

**Summary**:
이 문서는 Claude Code의 `.claude/rules/` 기능을 사용하여 `CLAUDE.md` 파일의 비대화를 방지하는 방법을 설명합니다. 이 기능은 규칙을 파일별로 분할하고 필요할 때만 동적으로 로드하여 컨텍스트 메모리 사용을 최적화합니다. `paths` 지정을 통해 특정 파일 작업 시에만 관련 규칙이 로드되도록 설정할 수 있으며, 여러 규칙의 동시 적용 및 중복 로드 방지 기능도 제공합니다. 이를 통해 프런트엔드/백엔드 규칙 분리, 테스트 파일 전용 규칙, 문서 작성 규칙 등 다양한 활용이 가능합니다.

**Date Created**: 2026-01-15