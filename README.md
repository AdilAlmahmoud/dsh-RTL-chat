# dsh-rtl-chat

A [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) (DSH) client plugin that makes the **conversation answer area read right-to-left (RTL)** for Arabic, with a one-click button to switch back to left-to-right (LTR).

يعرض منطقة إجابات المحادثة في DeepSeek Harness من اليمين إلى اليسار لقراءة العربية بشكل صحيح، مع زر للتبديل إلى الاتجاه العادي.

## Behavior / السلوك

- Only the message/answer scroll area of the conversation panel becomes RTL — the header, sidebar, and app chrome stay LTR.
  يُطبَّق الاتجاه على منطقة الرسائل فقط؛ الترويسة والشريط الجانبي تبقى كما هي.
- Code blocks (`pre`/`code`) inside answers stay LTR so code remains readable.
- A pill button (◄► **RTL**/**LTR**) appears in the conversation header. Clicking it toggles the direction; your choice is persisted in `localStorage` and survives reloads.
  زر دائري في ترويسة المحادثة يبدّل الاتجاه، ويُحفظ اختيارك تلقائيًا.

## How it works / آلية العمل

- Registers a toggle button into the live slot `conversation.header.leading` via `ctx.slots.inject` (client-only; no agent tool involved).
- Applies a scoped stylesheet (`[data-conversation-scroll]{direction:rtl}` + LTR guards for code) through the package-owned `styles.insert`, inserted/removed on toggle.
- Uses only `--dsw-alias-*` theme tokens; no imports from Harness client packages.

## Install / التثبيت

Requires a DSH web client (profile with `@deepseek-ai/dsh-web-app`) where the conversation slot tree is available.

1. Copy this folder anywhere on disk, e.g. `D:\plugins\dsh-rtl-chat`.
2. In a DSH agent session, ask it to install the bundle:
   ```
   plugin_manager → install_bundle → target: <absolute path to dsh-rtl-chat>
   ```
   or manually: add `"@local/dsh-rtl-chat": "link:<absolute path>"` to the profile's `package.json` dependencies **and** to `dsh.profile.bundles`, then run `pnpm install` in the profile directory.
3. Refresh the Web UI once. The button appears in the conversation header and RTL is on by default.

> Note: replacing an already-installed copy of this package requires a DSH server restart; new installs activate live.

## Files / الملفات

| File | Purpose |
| --- | --- |
| `package.json` | Bundle manifest (client half, web platform, injects into the conversation UI) |
| `cordis.patch.yml` | Loader row that inserts this bundle into a profile composition |
| `index.js` | Host half (no-op; all state lives in the client module) |
| `client.js` | Client half: toggle button, direction state, scoped RTL styles |
| `icon.svg` | Plugin icon shown in manager UIs |
| `locale/en.json`, `locale/ar.json` | Display metadata (title/description) |

## License / الرخصة

MIT
