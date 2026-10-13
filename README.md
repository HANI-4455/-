# Claude Code 디자인·에이전트 툴킷

사진 속 MCP·스킬·플러그인을 이 저장소에 연결해 둔 설정입니다.
이 폴더에서 `claude`를 실행하면 아래 항목이 자동으로 잡힙니다.

## 연결된 항목

| # | 이름 | 종류 | 연결 방식 | 비용 |
|---|------|------|-----------|------|
| 1 | Nicelydone | MCP (실제 서비스 화면·플로우·컴포넌트) | `.mcp.json` → `https://mcp.nicelydone.club/mcp` (OAuth) | 유료 플랜 |
| 2 | Refero | MCP + 스킬 (`refero-design`) | 플러그인 `refero@refero` → `https://api.refero.design/mcp` (OAuth) | 유료 플랜 |
| 3 | Lazyweb | MCP + 스킬 (`/lazyweb-search-screens` 등) | `.mcp.json` → `https://www.lazyweb.com/mcp` (토큰) + `.claude/skills/lazyweb*` | 설치 무료, 전체 리포트 Pro |
| 4 | Inspo AI | MCP (`find_website_inspiration`, `search_icons`, `extract_brand`, `list_design_systems`, `generate_ui`) | `.mcp.json` → `https://app.inspoai.io/mcp` (OAuth) | 무료 월 15회 |
| 5 | Ruflo | 멀티 에이전트 오케스트레이션 + MCP | 플러그인 `ruflo-core@ruflo` | 무료 (MIT) |
| 6 | Arena | 스킬 (`/arena-skill:arena`, 여러 버전을 토너먼트로 경쟁) | 플러그인 `arena-skill@arena-skill` | 무료 (MIT) |
| 7 | frontend-design | 스킬 (Anthropic 공식) | 플러그인 `frontend-design@claude-plugins-official` | 무료 |
| 8 | Impeccable | 디자인 스킬 (`/impeccable ...`) | 플러그인 `impeccable@impeccable` | 무료 (Apache-2.0) |
| 9 | Awesome Design MD | `DESIGN.md` 모음 85개 | `design-md/` 폴더 | 무료 (MIT) |

## 처음 한 번만 할 일

1. **Lazyweb 토큰 발급** (터미널에서):
   ```bash
   curl -fsSL https://www.lazyweb.com/install.sh | bash
   export LAZYWEB_MCP_TOKEN="$(cat ~/.lazyweb/lazyweb_mcp_token)"
   ```
   `export` 줄은 `~/.zshrc` 또는 `~/.bashrc`에 넣어 두세요. 토큰은 절대 커밋하지 마세요.
2. 이 폴더에서 `claude` 실행 → 프로젝트 MCP 서버(nicelydone, lazyweb, inspoai)와 플러그인 마켓플레이스 설치를 **승인**합니다.
3. Claude Code 안에서 `/mcp` 를 열고 **Nicelydone · Refero · Inspo AI** 를 각각 선택해 브라우저로 로그인(OAuth)합니다.
4. `/plugin` 에서 플러그인 5개가 설치·활성화됐는지 확인합니다.

## 사용 예시

- `pricing page annual monthly toggle 실제 화면 찾아서 요금제 페이지 만들어줘` → Refero / Nicelydone / Lazyweb 검색
- `/lazyweb-search-flows 온보딩` · `/lazyweb-search-experiments paywall`
- `stripe.com 브랜드 색과 글꼴 뽑아줘` → Inspo AI `extract_brand`
- `design-md/linear.app/DESIGN.md 스타일로 대시보드 만들어줘`
- `/arena-skill:arena 이 함수 리팩터링` · `/impeccable polish`
- Ruflo: `/ruflo` (콘솔), 자세한 기능은 [ruvnet/ruflo](https://github.com/ruvnet/ruflo)

## 현재 상태: 디자인 도구는 꺼 둠

지금은 **Arena, Ruflo만 켜져 있고** 화면 디자인용 도구는 꺼져 있습니다.
화면(웹페이지, 대시보드 등)을 만들 일이 생기면 Claude Code에 이렇게 말하면 됩니다:

> README의 "디자인 도구 다시 켜기" 대로 다시 켜줘

### 디자인 도구 다시 켜기

1. `.claude/settings.json`
   - `enabledPlugins`에서 `refero@refero`, `impeccable@impeccable`, `frontend-design@claude-plugins-official`을 `true`로 변경
   - `disabledMcpjsonServers`를 `[]`로, `enabledMcpjsonServers`를 `["nicelydone", "lazyweb", "inspoai"]`로 변경
2. Lazyweb 스킬을 쓰려면 `optional/lazyweb-skills/` 폴더를 `.claude/skills/`로 이동
3. Claude Code를 다시 시작하고 `/mcp`에서 로그인

## 참고

- 플러그인은 `.claude/settings.json`의 마켓플레이스에서 받아오므로 항상 최신 버전이 설치됩니다.
- Lazyweb 스킬은 `aboul3ata/lazyweb-skill`에서 복사했습니다. `lazyweb-design-create`의 일부 기능(브라우저 캡처)은 공식 설치 스크립트로 `~/.lazyweb`가 설치돼 있어야 동작합니다.
- 유료 MCP(Nicelydone, Refero)는 플랜이 없으면 로그인 후에도 도구 호출이 거부됩니다.
