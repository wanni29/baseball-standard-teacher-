# 야구 프로그램

## 기능정리
- 야구장 등록?name=잠실야구장
- 야구장목록
- 팀등록?stadiumId=1&name=NC
- 팀목록
```text
  Stadium 조인해서 가져오기
```

- 선수등록?teamId=1&name=이대호&position=1루수
- 선수목록?teamId=1
- 퇴출등록?playerId=1&reason=도박 
```text 
out_player에 퇴출 선수를 insert하고, player 테이블에서 해당 선수의 team_id를 null로 변경합니다.
```  
- 퇴출목록
```text
player 테이블과 out_player 테이블이 outerJoin 되면된다.
player가 driving 이라면 left outer join
```
-포지션별목록
```text
피벗
```

## 단계
-1단계
```text
야구장등록, 야구장목록, 팀등록, 팀목록 ,선수등록 ,선수목록 ,퇴출등록 ,퇴출목록
```

-2단계
```text
포지션별목록
```

-3단계
```text
테스트 - 예외처리
```

-4단계
```text
UX 사용자 경험을 좋게 해주기
```


## 시나리오 데이터 쿼리 만들기
```sql
-- 야구장등록?name=창원NC파크
insert into stadium(name, created_at) values('창원NC파크', now());

-- 야구장목록
select *
from stadium;

-- 팀등록?stadiumId=4&name=NC
insert into team(stadium_id, name, created_at) values(4, 'NC', now());

-- 팀목록
select t.id t_id, s.name s_name, t.name t_name, t.created_at t_created_at
from team t inner join stadium s on t.stadium_id = s.id;

-- 선수등록?teamId=4&name=심창민&position=투수
insert into player (team_id, name, position, created_at) 
values(4, '심창민', '투수', now());

-- 선수목록?teamId=1
select * from player where team_id=1;

-- 퇴출등록?playerId=8&reason=도박
insert into out_player(player_id, reason, created_at) 
values(8, '도박', now());
update player set team_id = null where id = 8;

-- 퇴출목록
select 
p.id p_id, 
p.name p_name, 
p.position p_position, 
op.reason op_reason,
op.created_at op_created_at
from player p left outer join out_player op
on p.id = op.player_id;

-- 포지션별목록

```



## 스키마 
```sql
create database baseball;

use baseball;

create table stadium (
	id int primary key auto_increment,
    name varchar(250) not null,
    created_at timestamp
) DEFAULT CHARACTER SET utf8mb4;

create table team (
	id int primary key auto_increment,
    stadium_id int,
    name varchar(250) not null,
    created_at timestamp,
    FOREIGN KEY (stadium_id) REFERENCES stadium(id)
) DEFAULT CHARACTER SET utf8mb4;

create table player (
	id int primary key auto_increment,
    team_id int,
    name varchar(250) not null,
	position varchar(250) not null,
    created_at timestamp,
    FOREIGN KEY (team_id) REFERENCES team(id),
    UNIQUE (team_id, position)
) DEFAULT CHARACTER SET utf8mb4;

create table out_player (
	id int primary key auto_increment,
    player_id int,
    reason varchar(250) not null,
    created_at timestamp,
    FOREIGN KEY (player_id) REFERENCES player(id)
) DEFAULT CHARACTER SET utf8mb4;
```

## 더미 데이터
```sql
insert into stadium(name, created_at) values('사직야구장', now());
insert into stadium(name, created_at) values('잠실야구장', now());
insert into stadium(name, created_at) values('라이온즈파크', now());

insert into team(stadium_id, name, created_at) values(1, '롯데', now());
insert into team(stadium_id, name, created_at) values(2, '두산', now());
insert into team(stadium_id, name, created_at) values(3, '삼성', now());

INSERT INTO player (team_id, name, position, created_at) VALUES
-- 롯데 자이언츠 선수
(1, '손아섭', '1루수', now()),
(1, '이대호', '2루수', now()),
(1, '임병욱', '3루수', now()),
(1, '김재현', '투수', now()),
(1, '윌슨즈', '포수', now()),
(1, '김문호', '유격수', now()),
(1, '윌린오', '좌익수', now()),
(1, '전준우', '중견수', now()),
(1, '김용의', '우익수', now()),
-- 삼성 라이온즈 선수
(2, '이원석', '1루수', now()),
(2, '김동엽', '2루수', now()),
(2, '김상수', '3루수', now()),
(2, '구자욱', '투수', now()),
(2, '이학주', '포수', now()),
(2, '박해민', '유격수', now()),
(2, '강민호', '좌익수', now()),
(2, '박경수', '중견수', now()),
(2, '조동찬', '우익수', now()),
-- 두산 베어스 선수
(3, '김재호', '1루수', now()),
(3, '오재일', '2루수', now()),
(3, '페르난', '3루수', now()),
(3, '정수빈', '투수', now()),
(3, '김재환', '포수', now()),
(3, '김인태', '유격수', now()),
(3, '오재원', '좌익수', now()),
(3, '박건우', '중견수', now()),
(3, '호잉요', '우익수', now()),
```































