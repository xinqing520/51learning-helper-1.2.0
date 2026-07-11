# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

A single-file Tampermonkey userscript that injects a control panel into the [51Learning](http://reading.51learning.com.cn:8080/) reading platform. It helps users collect article queues, manage reading timers, copy questions to external AI tools, and auto-fill answers.

## Build / Test

There is no build step. Edit `51learning-helper.user.js` directly, then import it into Tampermonkey's dashboard for local debugging. Open the target website (`http://reading.51learning.com.cn:8080/Reading/*`) to test.

## Architecture

Single IIFE (`"use strict"`) ~1640 lines in one file. No external dependencies. No bundler, no package manager.

### Data layer (`localStorage`)

All keys are prefixed with `cl51wf:` (the `APP` constant). Key patterns:

- **Config**: `cl51wf:config` — reading mode, speed, book/unit IDs. Merged with `DEFAULT_CONFIG` on load.
- **Queue**: `cl51wf:queue:{b}:{u}` — per-unit article list (collected by fetching and parsing list pages).
- **Done**: `cl51wf:done:{b}:{u}` — per-unit read-status map.
- **Position**: `cl51wf:position:{b}:{u}` — current index within a unit's queue.
- **Timer**: `cl51wf:timer:{b}:{u}:{passageId}` — running reading timer (`startedAt`, `targetMs`, `notified` flag).
- **Timer paused**: `cl51wf:timerPaused:{b}:{u}:{passageId}` — flag set when timer is cleared.
- **Answer DB**: `cl51wf:answerDb` — saved answers keyed by `passageId`.

### Page types

- **List page** (`/Reading/Articles`): Collect article queue by fetching all pages via `fetchDocument()` + `DOMParser`, parse `.blog-grid` cards. Falls back to probing pages when total count is unknown.
- **Article page** (`/Reading/ArticleItem`): Reading timer, mode selection, question extraction, answer fill.

### Page state detection (`detectCurrentPageSiteStatus`)

Returns one of: `"未开始"` | `"阅读中"` | `"已读完"` | `"待提交"` | `"已完成"`. Detection is based on visible button text (开始阅读/完成阅读/提交答案), page text patterns, and timer state. The timer UI behavior changes based on this state.

### Question field extraction (`questionFields`)

Finds all answer inputs on the page by searching for `questions[\d+].subQuestions[\d+].userAnswerText` named fields (hidden inputs, textareas, text inputs, radio groups). Groups them by task, infers task type (vocabulary/matching/multiple_choice/translation/critical_thinking/writing) from task title text patterns.

### Answer parsing (`parseAnswers`)

Accepts multiple formats:
1. JSON (parsed directly, supports nested section/byQuestionId/flatAnswers structures)
2. Simple template format (markdown-like with `【section name】` headers and numbered lines)
3. Key=value lines (`1: A`)
4. Whitespace/comma-delimited flat list

### Timer system

- `startReading()`: Clicks mode buttons, clicks site's "开始阅读", estimates reading time from word count / WPM with ±15% jitter.
- `updateTimerUi()`: 1-second interval ticker. Monitors elapsed time, auto-clicks "完成阅读" when target reached. Syncs with site's own timer display. Detects stale timers (user navigated away while timer ran).
- `adoptSiteTimerIfNeeded()`: If the site already shows reading in progress but no local timer exists, creates one synced to the site's elapsed time.

### Element finding strategy

Uses a layered approach to find interactive elements on a dynamic page:
1. `findByText()` — search all visible candidates by exact/contains text match
2. `findActionByText()` — narrower search on clickable elements only
3. `findDirectActionByText()` — direct match on clickable elements (no ancestor fallback)
4. `robustClick()` — dispatches multiple mouse events + calls `.click()` for maximum compatibility

### UI injection

`injectUi()` builds a fixed-position panel (`#cl51-panel`, z-index 2147483647) at top-right of the page. All DOM queries filter out helper elements via `isHelperElement()` to avoid the panel interfering with site element detection. The close button removes the panel from DOM entirely.

## Key constants

- `DEFAULT_CONFIG`: b="7", u="71", limit=10, displayMode="完整阅读", revealMode="逐段", speedMode="中(每分钟100个)", minMinutes=5, maxMinutes=7
- Timer estimation: word count from `#articleContent` text, divided by WPM from speed mode string
- The `@match` directive restricts the script to `http://reading.51learning.com.cn:8080/Reading/*`
