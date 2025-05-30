# 🐛 MCP 서버 설정 버그 리포트

> **날짜**: 2025-05-30  
> **작성자**: kimhxsong  
> **상태**: 해결됨  
> **카테고리**: Configuration Bug

## 📋 문제 요약

MCP 서버 설정에서 잘못된 `--server` 인수 사용 시 조용한 실패(Silent Failure)가 발생하는 문제입니다.

## 🔍 문제 상황

- **환경**: Cursor IDE + cursor-talk-to-figma-mcp
- **증상**: `channel_join`은 성공하지만 실제 MCP 도구 호출은 실패
- **원인**: 복사한 설정 예제에 포함된 잘못된 서버 인수

## 🔄 재현 단계

1. **잘못된 MCP 설정**:
```json
{
  "mcpServers": {
    "TalkToFigma": {
      "command": "bunx",
      "args": [
        "cursor-talk-to-figma-mcp@latest",
        "--server=vps.sonnylab.com"  // ← 이 부분이 문제!
      ]
    }
  }
}
```

2. Cursor 실행 후 agent 모드에서 MCP 도구 사용 시도
3. `channel_join`은 성공한 것처럼 보임
4. 하지만 실제 도구 호출은 조용히 실패

## ✅ 해결 방법

**잘못된 서버 인수 제거**:
```json
{
  "mcpServers": {
    "TalkToFigma": {
      "command": "bunx", 
      "args": ["cursor-talk-to-figma-mcp@latest"]
    }
  }
}
```

## 💡 핵심 교훈

1. **설정 복사 시 주의**: 예제 설정을 복사할 때 환경별 인수 확인 필요
2. **오류 처리 개선**: MCP 도구에서 더 명확한 오류 메시지 제공 필요
3. **설정 검증**: 시작 시 설정 유효성 검사 로직 개선 필요

## 🔗 관련 리소스

- [cursor-talk-to-figma-mcp GitHub](https://github.com/kimhxsong/cursor-talk-to-figma-mcp)
- [Cursor MCP 문서](https://docs.cursor.com/context/model-context-protocol)

## 📝 메타데이터

| 항목 | 값 |
|------|-----|
| 심각도 | 중간 |
| 빈도 | 흔함 (설정 복사 시) |
| 영향 | 디버깅 어려움, 시간 낭비 |
| 해결 시간 | 복붙 실수 발견 후 즉시 |

---

*이 버그 리포트는 [kimhxsong.github.io](https://kimhxsong.github.io)에서 관리됩니다.*

<!-- 
작성 목적: 비슷한 문제를 겪는 개발자들에게 도움이 되기 위함
키워드: MCP, Cursor, Configuration, Silent Failure, Debugging
-->
