# Location Tracking Service Design

## Objective

- 위치 추적 서비스는 주기적으로 유저의 위치 데이터를 수집해 자주 방문하는 위치를 식별

## Service Overview

- 주요 기능:
    - 이동 거리 기반 위치 수집
    - 체류 시간 계산 및 자주 방문하는 위치 도출
    - 유저 정보 업데이트

## Data Flow

- 데이터 수집:
    - 모바일 앱에서 위치 데이터를 수집하여 API 서버로 전송
- 데이터 변환:
    - 위,경도를 "Lambert Conformal Conic Projection"을 통해 격자쌍으로 변환
    - 격자에 따라 체류시간, 가장 마지막에 방문한 시간을 계산
- 데이터 저장

```shell
user_grid_visits
+--------------+---------------------+------------------+----------------------+
| user_id     | grid_x, grid_y       | dwell_time       | last_visit_time      |
+--------------+---------------------+------------------+----------------------+
| user_001    | (60, 127)            | 15 min           | 2025-01-14 08:00     |
| user_001    | (61, 128)            | 30 min           | 2025-01-13 18:30     |
| user_002    | (60, 127)            | 10 min           | 2025-01-14 09:00     |
+--------------+---------------------+------------------+----------------------+
```
