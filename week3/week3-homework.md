week3 homework
================
2022-07-29

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✔ ggplot2 3.3.6     ✔ purrr   0.3.4
    ## ✔ tibble  3.1.7     ✔ dplyr   1.0.9
    ## ✔ tidyr   1.2.0     ✔ stringr 1.4.0
    ## ✔ readr   2.1.2     ✔ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(nycflights13)
```

dplyr로 하는 관계형데이터

**R은 데이터 분석 언어**이기 때문에 일부 기능들은 데이터베이스를
관리하는 프로그램과 유사하고 관련이 있다.

여러 데이터 테이블을 총칭하여 **관계형 데이터**라고 한다.개별 데이터셋이
아니라 이들의 관계가 중요한 것이기 때문이다.

-   뮤테이팅 조인 : 다른 데이터프레임에 있는 관측값에서 가져와 새로운
    변수로 생성하여 추가

-   필터링 조인 : 다른 테이블의 관측값과 일치하는지에 따라 관측값을
    걸러냄

-   집합 연산 : 관측값을 집합 원소로 취급

``` r
# flights라는 데이터는 서로 다른 데이터셋에서 가져와서 만들어진 관계형 데이터.

str(airlines) # carrier 약어 코드로 항공사명을 찾아볼 수 있다.
```

    ## tibble [16 × 2] (S3: tbl_df/tbl/data.frame)
    ##  $ carrier: chr [1:16] "9E" "AA" "AS" "B6" ...
    ##  $ name   : chr [1:16] "Endeavor Air Inc." "American Airlines Inc." "Alaska Airlines Inc." "JetBlue Airways" ...

``` r
str(airports) # faa 공항코드
```

    ## tibble [1,458 × 8] (S3: tbl_df/tbl/data.frame)
    ##  $ faa  : chr [1:1458] "04G" "06A" "06C" "06N" ...
    ##  $ name : chr [1:1458] "Lansdowne Airport" "Moton Field Municipal Airport" "Schaumburg Regional" "Randall Airport" ...
    ##  $ lat  : num [1:1458] 41.1 32.5 42 41.4 31.1 ...
    ##  $ lon  : num [1:1458] -80.6 -85.7 -88.1 -74.4 -81.4 ...
    ##  $ alt  : num [1:1458] 1044 264 801 523 11 ...
    ##  $ tz   : num [1:1458] -5 -6 -6 -5 -5 -5 -5 -5 -5 -8 ...
    ##  $ dst  : chr [1:1458] "A" "A" "A" "A" ...
    ##  $ tzone: chr [1:1458] "America/New_York" "America/Chicago" "America/Chicago" "America/New_York" ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   id = col_double(),
    ##   ..   name = col_character(),
    ##   ..   city = col_character(),
    ##   ..   country = col_character(),
    ##   ..   faa = col_character(),
    ##   ..   icao = col_character(),
    ##   ..   lat = col_double(),
    ##   ..   lon = col_double(),
    ##   ..   alt = col_double(),
    ##   ..   tz = col_double(),
    ##   ..   dst = col_character(),
    ##   ..   tzone = col_character()
    ##   .. )

``` r
str(planes) # tailnum으로 식별되어 있다.
```

    ## tibble [3,322 × 9] (S3: tbl_df/tbl/data.frame)
    ##  $ tailnum     : chr [1:3322] "N10156" "N102UW" "N103US" "N104UW" ...
    ##  $ year        : int [1:3322] 2004 1998 1999 1999 2002 1999 1999 1999 1999 1999 ...
    ##  $ type        : chr [1:3322] "Fixed wing multi engine" "Fixed wing multi engine" "Fixed wing multi engine" "Fixed wing multi engine" ...
    ##  $ manufacturer: chr [1:3322] "EMBRAER" "AIRBUS INDUSTRIE" "AIRBUS INDUSTRIE" "AIRBUS INDUSTRIE" ...
    ##  $ model       : chr [1:3322] "EMB-145XR" "A320-214" "A320-214" "A320-214" ...
    ##  $ engines     : int [1:3322] 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ seats       : int [1:3322] 55 182 182 182 55 182 182 182 182 182 ...
    ##  $ speed       : int [1:3322] NA NA NA NA NA NA NA NA NA NA ...
    ##  $ engine      : chr [1:3322] "Turbo-fan" "Turbo-fan" "Turbo-fan" "Turbo-fan" ...

``` r
str(weather) # 날씨 정보를 가지고 있는 데이터셋
```

    ## tibble [26,115 × 15] (S3: tbl_df/tbl/data.frame)
    ##  $ origin    : chr [1:26115] "EWR" "EWR" "EWR" "EWR" ...
    ##  $ year      : int [1:26115] 2013 2013 2013 2013 2013 2013 2013 2013 2013 2013 ...
    ##  $ month     : int [1:26115] 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ day       : int [1:26115] 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ hour      : int [1:26115] 1 2 3 4 5 6 7 8 9 10 ...
    ##  $ temp      : num [1:26115] 39 39 39 39.9 39 ...
    ##  $ dewp      : num [1:26115] 26.1 27 28 28 28 ...
    ##  $ humid     : num [1:26115] 59.4 61.6 64.4 62.2 64.4 ...
    ##  $ wind_dir  : num [1:26115] 270 250 240 250 260 240 240 250 260 260 ...
    ##  $ wind_speed: num [1:26115] 10.36 8.06 11.51 12.66 12.66 ...
    ##  $ wind_gust : num [1:26115] NA NA NA NA NA NA NA NA NA NA ...
    ##  $ precip    : num [1:26115] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ pressure  : num [1:26115] 1012 1012 1012 1012 1012 ...
    ##  $ visib     : num [1:26115] 10 10 10 10 10 10 10 10 10 10 ...
    ##  $ time_hour : POSIXct[1:26115], format: "2013-01-01 01:00:00" "2013-01-01 02:00:00" ...

-   flights는 carrier를 통해 airlines에 연결 됨

-   flights는 faa와 dest(목적지) 변수를 통해 airports에 연결 됨

-   flights는 tailnum을 통해 planes와 연결 됨

-   flights는 origin(위치), year, month, day,hour를 통해 weather과 연결
    됨

# 1. 키(key)

고유하게 식별할 수 있는 **변수 또는 변수의 집합**을 키라고 부른다.

다양한 변수를 조합해야 고유하게 식별할 수 있는 경우 또한 존재한다.

-   기본키(primary key) : 자신의 테이블에서 관측값을 고유하게 식별한다.

-   외래키(foreign key) : 다른 테이블의 관측값을 고유하게 식별한다.

#### 한 테이블의 변수가 다른 테이블에서 키의 역할을 할 수 있다면 다른 테이블에서 외래키라고 부른다.

``` r
# 데이터를 고유하게 식별하는 지 확인하는 방법
planes %>%
  count(tailnum) %>%
  filter(n > 1)
```

    ## # A tibble: 0 × 2
    ## # … with 2 variables: tailnum <chr>, n <int>

``` r
# 결과값으로 아무것도 안나옴
# 이런 걸 key라고 부름(tailnum)
```

mutate(), row_number() 이용하여 기본키를 추가할 수 있다. 이를
**대체키(surrogate key)** 라고 부른다.

``` r
# 대체key 추가하고 key가 맨 앞에 오도록 설정
# select 이용해서 변수 순서 바꿀 수 있음.

flights %>%
  dplyr::mutate(key = row_number()) %>%
  dplyr::select(key, everything()) %>% head()
```

    ## # A tibble: 6 × 20
    ##     key  year month   day dep_time sched_dep_time dep_delay arr_time
    ##   <int> <int> <int> <int>    <int>          <int>     <dbl>    <int>
    ## 1     1  2013     1     1      517            515         2      830
    ## 2     2  2013     1     1      533            529         4      850
    ## 3     3  2013     1     1      542            540         2      923
    ## 4     4  2013     1     1      544            545        -1     1004
    ## 5     5  2013     1     1      554            600        -6      812
    ## 6     6  2013     1     1      554            558        -4      740
    ## # … with 12 more variables: sched_arr_time <int>, arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

# 2. 뮤테이팅 조인

두 데이터 테이블의 변수를 결합할 수 있다.

``` r
# carrier를 기준으로 항공사 이름(name)을 flights2에 추가하려고 한다.
flights2 <- flights %>%
  dplyr::select(year:day, hour, origin, dest, tailnum, carrier)

str(flights2)
```

    ## tibble [336,776 × 8] (S3: tbl_df/tbl/data.frame)
    ##  $ year   : int [1:336776] 2013 2013 2013 2013 2013 2013 2013 2013 2013 2013 ...
    ##  $ month  : int [1:336776] 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ day    : int [1:336776] 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ hour   : num [1:336776] 5 5 5 5 6 5 6 6 6 6 ...
    ##  $ origin : chr [1:336776] "EWR" "LGA" "JFK" "JFK" ...
    ##  $ dest   : chr [1:336776] "IAH" "IAH" "MIA" "BQN" ...
    ##  $ tailnum: chr [1:336776] "N14228" "N24211" "N619AA" "N804JB" ...
    ##  $ carrier: chr [1:336776] "UA" "UA" "AA" "B6" ...

``` r
# 원래는 left_join(flights2, airlines, by = "carrier")
# pipe로 생략
flights2 %>%
  dplyr::select(-origin, -dest) %>%
  left_join(airlines, by = "carrier") %>% # carrier기준으로 합침
  filter(is.na(name) == T) # 무결점 검사
```

    ## # A tibble: 0 × 7
    ## # … with 7 variables: year <int>, month <int>, day <int>, hour <dbl>,
    ## #   tailnum <chr>, carrier <chr>, name <chr>

# 3. 내부 조인(inner-join)과 외부 조인(outer-join)

내부 조인은 키가 같을 때 마다 두 관측값을 매칭한다.(잘 안쓴다)

외부 조인은 관측값을 모두 보존한다.

## (1) 내부 조인

``` r
x <- tibble(key = c(1:3),
            val_x = c("x1", "x2", "x3"))

y <- tibble(key = c(1,2,4),
            val_y = c("y1", "y2", "y3"))

# 내부조인
dplyr::inner_join(x, y, by = "key")
```

    ## # A tibble: 2 × 3
    ##     key val_x val_y
    ##   <dbl> <chr> <chr>
    ## 1     1 x1    y1   
    ## 2     2 x2    y2

## (2) 외부 조인

``` r
# 외부 조인 (1) : left join
x <- tibble(key = c(1, 2, 2, 1),
            val_x = c("x1", "x2", "x3", "x4"))

y <- tibble(key = c(1,2),
            val_y = c("y1", "y2"))

left_join(x, y, by = "key")
```

    ## # A tibble: 4 × 3
    ##     key val_x val_y
    ##   <dbl> <chr> <chr>
    ## 1     1 x1    y1   
    ## 2     2 x2    y2   
    ## 3     2 x3    y2   
    ## 4     1 x4    y1

``` r
# 외부 조인 (2) : right join
# 두 데이터 모두 중복 키가 있는 경우
x <- tibble(key = c(1,2,2,3),
            val_x = c("x1", "x2", "x3", "x4"))

y <- tibble(key = c(1,2,2,3),
            val_y = c("y1", "y2", "y3", "y4"))

right_join(x, y, by = "key")
```

    ## # A tibble: 6 × 3
    ##     key val_x val_y
    ##   <dbl> <chr> <chr>
    ## 1     1 x1    y1   
    ## 2     2 x2    y2   
    ## 3     2 x2    y3   
    ## 4     2 x3    y2   
    ## 5     2 x3    y3   
    ## 6     3 x4    y4

``` r
# 외부 조인 (3) : full join
x <- tibble(key = c(1,2,3),
            val_x = c("x1", "x2", "x3"))

y <- tibble(key = c(1,2,4),
            val_y = c("y1", "y2", "y3"))

full_join(x, y, by = "key")
```

    ## # A tibble: 4 × 3
    ##     key val_x val_y
    ##   <dbl> <chr> <chr>
    ## 1     1 x1    y1   
    ## 2     2 x2    y2   
    ## 3     3 x3    <NA> 
    ## 4     4 <NA>  y3

# 4. 키 열 정의하기

by = “key”를 통하여 조인을 하였지만, 다른 방법으로 데이터 테이블을
연결할 수도 있다.

-   기본값을 by = NULL로 할 경우, 모든 변수를 사용하며, 이를 natural
    join이라고 부른다.

``` r
# weather의 모든 변수를 사용해서 join을 하게 됨.
flights2 %>% 
  left_join(weather) %>% head() # 순서쌍 조합으로 생성
```

    ## Joining, by = c("year", "month", "day", "hour", "origin")

    ## # A tibble: 6 × 18
    ##    year month   day  hour origin dest  tailnum carrier  temp  dewp humid
    ##   <int> <int> <int> <dbl> <chr>  <chr> <chr>   <chr>   <dbl> <dbl> <dbl>
    ## 1  2013     1     1     5 EWR    IAH   N14228  UA       39.0  28.0  64.4
    ## 2  2013     1     1     5 LGA    IAH   N24211  UA       39.9  25.0  54.8
    ## 3  2013     1     1     5 JFK    MIA   N619AA  AA       39.0  27.0  61.6
    ## 4  2013     1     1     5 JFK    BQN   N804JB  B6       39.0  27.0  61.6
    ## 5  2013     1     1     6 LGA    ATL   N668DN  DL       39.9  25.0  54.8
    ## 6  2013     1     1     5 EWR    ORD   N39463  UA       39.0  28.0  64.4
    ## # … with 7 more variables: wind_dir <dbl>, wind_speed <dbl>, wind_gust <dbl>,
    ## #   precip <dbl>, pressure <dbl>, visib <dbl>, time_hour <dttm>

``` r
flights2 %>%
  left_join(planes, by = "tailnum") %>% head()
```

    ## # A tibble: 6 × 16
    ##   year.x month   day  hour origin dest  tailnum carrier year.y type             
    ##    <int> <int> <int> <dbl> <chr>  <chr> <chr>   <chr>    <int> <chr>            
    ## 1   2013     1     1     5 EWR    IAH   N14228  UA        1999 Fixed wing multi…
    ## 2   2013     1     1     5 LGA    IAH   N24211  UA        1998 Fixed wing multi…
    ## 3   2013     1     1     5 JFK    MIA   N619AA  AA        1990 Fixed wing multi…
    ## 4   2013     1     1     5 JFK    BQN   N804JB  B6        2012 Fixed wing multi…
    ## 5   2013     1     1     6 LGA    ATL   N668DN  DL        1991 Fixed wing multi…
    ## 6   2013     1     1     5 EWR    ORD   N39463  UA        2012 Fixed wing multi…
    ## # … with 6 more variables: manufacturer <chr>, model <chr>, engines <int>,
    ## #   seats <int>, speed <int>, engine <chr>

``` r
str(flights2)
```

    ## tibble [336,776 × 8] (S3: tbl_df/tbl/data.frame)
    ##  $ year   : int [1:336776] 2013 2013 2013 2013 2013 2013 2013 2013 2013 2013 ...
    ##  $ month  : int [1:336776] 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ day    : int [1:336776] 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ hour   : num [1:336776] 5 5 5 5 6 5 6 6 6 6 ...
    ##  $ origin : chr [1:336776] "EWR" "LGA" "JFK" "JFK" ...
    ##  $ dest   : chr [1:336776] "IAH" "IAH" "MIA" "BQN" ...
    ##  $ tailnum: chr [1:336776] "N14228" "N24211" "N619AA" "N804JB" ...
    ##  $ carrier: chr [1:336776] "UA" "UA" "AA" "B6" ...

``` r
str(airports)
```

    ## tibble [1,458 × 8] (S3: tbl_df/tbl/data.frame)
    ##  $ faa  : chr [1:1458] "04G" "06A" "06C" "06N" ...
    ##  $ name : chr [1:1458] "Lansdowne Airport" "Moton Field Municipal Airport" "Schaumburg Regional" "Randall Airport" ...
    ##  $ lat  : num [1:1458] 41.1 32.5 42 41.4 31.1 ...
    ##  $ lon  : num [1:1458] -80.6 -85.7 -88.1 -74.4 -81.4 ...
    ##  $ alt  : num [1:1458] 1044 264 801 523 11 ...
    ##  $ tz   : num [1:1458] -5 -6 -6 -5 -5 -5 -5 -5 -5 -8 ...
    ##  $ dst  : chr [1:1458] "A" "A" "A" "A" ...
    ##  $ tzone: chr [1:1458] "America/New_York" "America/Chicago" "America/Chicago" "America/New_York" ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   id = col_double(),
    ##   ..   name = col_character(),
    ##   ..   city = col_character(),
    ##   ..   country = col_character(),
    ##   ..   faa = col_character(),
    ##   ..   icao = col_character(),
    ##   ..   lat = col_double(),
    ##   ..   lon = col_double(),
    ##   ..   alt = col_double(),
    ##   ..   tz = col_double(),
    ##   ..   dst = col_character(),
    ##   ..   tzone = col_character()
    ##   .. )

``` r
# flights2의 dest변수와 airports의 faa변수를 매칭시키는 경우
flights2 %>%
  left_join(airports, by = c("dest" = "faa")) %>% head()
```

    ## # A tibble: 6 × 15
    ##    year month   day  hour origin dest  tailnum carrier name      lat   lon   alt
    ##   <int> <int> <int> <dbl> <chr>  <chr> <chr>   <chr>   <chr>   <dbl> <dbl> <dbl>
    ## 1  2013     1     1     5 EWR    IAH   N14228  UA      George…  30.0 -95.3    97
    ## 2  2013     1     1     5 LGA    IAH   N24211  UA      George…  30.0 -95.3    97
    ## 3  2013     1     1     5 JFK    MIA   N619AA  AA      Miami …  25.8 -80.3     8
    ## 4  2013     1     1     5 JFK    BQN   N804JB  B6      <NA>     NA    NA      NA
    ## 5  2013     1     1     6 LGA    ATL   N668DN  DL      Hartsf…  33.6 -84.4  1026
    ## 6  2013     1     1     5 EWR    ORD   N39463  UA      Chicag…  42.0 -87.9   668
    ## # … with 3 more variables: tz <dbl>, dst <chr>, tzone <chr>

# 5. 필터링 조인 (굳이 쓸 필요는 없음)

semi_join(x, y) : y와 매치되는 x의 모든 관측값을 보존한다. anti_join(x,
y) : y와 매치되는 x의 모든 관측값을 삭제한다.

``` r
# 인기 있는 상위 10개의 도착지
top_dest <- flights %>%
  dplyr::count(dest, sort = TRUE) %>%
  head(10)

top_dest
```

    ## # A tibble: 10 × 2
    ##    dest      n
    ##    <chr> <int>
    ##  1 ORD   17283
    ##  2 ATL   17215
    ##  3 LAX   16174
    ##  4 BOS   15508
    ##  5 MCO   14082
    ##  6 CLT   14064
    ##  7 SFO   13331
    ##  8 FLL   12055
    ##  9 MIA   11728
    ## 10 DCA    9705

``` r
# 똑같은 표현
flights %>%
  dplyr::filter(dest %in% top_dest$dest) %>% head(10)
```

    ## # A tibble: 10 × 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      542            540         2      923            850
    ##  2  2013     1     1      554            600        -6      812            837
    ##  3  2013     1     1      554            558        -4      740            728
    ##  4  2013     1     1      555            600        -5      913            854
    ##  5  2013     1     1      557            600        -3      838            846
    ##  6  2013     1     1      558            600        -2      753            745
    ##  7  2013     1     1      558            600        -2      924            917
    ##  8  2013     1     1      558            600        -2      923            937
    ##  9  2013     1     1      559            559         0      702            706
    ## 10  2013     1     1      600            600         0      851            858
    ## # … with 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    ## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    ## #   hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
# 똑같은 표현
flights %>%
  semi_join(top_dest) %>% head(10) # 겹치는 값들만 보존
```

    ## Joining, by = "dest"

    ## # A tibble: 10 × 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      542            540         2      923            850
    ##  2  2013     1     1      554            600        -6      812            837
    ##  3  2013     1     1      554            558        -4      740            728
    ##  4  2013     1     1      555            600        -5      913            854
    ##  5  2013     1     1      557            600        -3      838            846
    ##  6  2013     1     1      558            600        -2      753            745
    ##  7  2013     1     1      558            600        -2      924            917
    ##  8  2013     1     1      558            600        -2      923            937
    ##  9  2013     1     1      559            559         0      702            706
    ## 10  2013     1     1      600            600         0      851            858
    ## # … with 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    ## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    ## #   hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
# anti_join / flights와 planes가 매칭이 되는 행을 삭제
flights %>%
  anti_join(planes, by = "tailnum") %>% # 겹치는 값들만 보존
  count(tailnum, sort = TRUE) %>% head(10)
```

    ## # A tibble: 10 × 2
    ##    tailnum     n
    ##    <chr>   <int>
    ##  1 <NA>     2512
    ##  2 N725MQ    575
    ##  3 N722MQ    513
    ##  4 N723MQ    507
    ##  5 N713MQ    483
    ##  6 N735MQ    396
    ##  7 N0EGMQ    371
    ##  8 N534MQ    364
    ##  9 N542MQ    363
    ## 10 N531MQ    349

# 6. 집합 연산

-   intersect(x, y) : x,y 모두에 있는 관측값만 반환

-   union(x, y) : x와 y의 고유한 관측값을 반환

-   setdiff(x, y) : x에 있지만 y에 없는 관측값을 반환

# 7. 문자열 기초

``` r
library(stringr) # tidyverse에 포함 된 패키지

# 문자열 길이 / 띄어쓰기도 포함
str_length(c("a", "R for data science", NA))
```

    ## [1]  1 18 NA

``` r
# 문자열 합치기
str_c("x", "y")
```

    ## [1] "xy"

``` r
str_c("x", "y", sep = ",")
```

    ## [1] "x,y"

``` r
str_c("x", "y", collapse = " ")
```

    ## [1] "xy"

``` r
# 문자열 NA값을 "NA" 대체하기 / 문자 NA로 변환
x <- c("abc", NA)
str_c("|-", str_replace_na(x), "-|")
```

    ## [1] "|-abc-|" "|-NA-|"

``` r
birthday <- TRUE
str_c("good morning", if (birthday) " and Happy Birthday")
```

    ## [1] "good morning and Happy Birthday"

``` r
# 문자열 서브셋하기
x <- c("apple", "banana", "pear")

str_sub(x, start = 1, end = 3)
```

    ## [1] "app" "ban" "pea"

``` r
str_sub(x, start = -3, end = -1)
```

    ## [1] "ple" "ana" "ear"

``` r
str_sub(x, start = 1, end = 1) <- str_to_upper(str_sub(x, 1, 1))
x
```

    ## [1] "Apple"  "Banana" "Pear"

``` r
# collapse
y <- c(1, 2, 3)
str_c(x, collapse = " ")
```

    ## [1] "Apple Banana Pear"

str_c는 paste(), paste0()와 유사하게 동작한다.

# 8. 정규표현식 - latex처럼 언어안의 언어(Cheatsheet 참고)

문자열 패턴을 기술하는 매우 간결한 언어이다.

## (1) 임의의 문자 매칭

. : 줄바꿈을 제외한 임의의 문자와 매칭한다.

``` r
x <- c("apple", "banana", "pear")
str_match(x, "an")
```

    ##      [,1]
    ## [1,] NA  
    ## [2,] "an"
    ## [3,] NA

``` r
str_match(x, ".a.")
```

    ##      [,1] 
    ## [1,] NA   
    ## [2,] "ban"
    ## [3,] "ear"

## (2) 앵커

^ : 문자열의 시작과 매칭 $ : 문자열의 끝과 매칭 단어사이의 경계를
매칭(공백)

``` r
# 해당 정규표현식과 매칭되는 애들을 찾아서 T/F
str_detect(x, "^a")
```

    ## [1]  TRUE FALSE FALSE

``` r
str_detect(x, "a$")
```

    ## [1] FALSE  TRUE FALSE

``` r
x <- c("applepie", "apple", "applecake")
str_detect(x, "apple") # 나는 apple이 들어간 거 찾을거야
```

    ## [1] TRUE TRUE TRUE

``` r
str_detect(x, "^apple$") # 나는 순수 apple만 찾을거야
```

    ## [1] FALSE  TRUE FALSE

## (3) 문자 클래스와 대체 구문

|                         |                                                  |
|:-----------------------:|:------------------------------------------------:|
| 임의의 숫자와 매칭한다. |                                                  |
|                         | 임의의 여백문자(공백, 탭, 줄바꿈 등)와 매치한다. |
|         \[abc\]         |             a, b 또는 c와 매치한다.              |
|        \[^abc\]         | a, b 또는 c를 **제외한** 임의의 문자와 매치한다. |
|           abc           |                       xyz                        |

연산의 우선순위는 괄호를 사용하여 결정

``` r
x <-c("ding","dina","dize")
y <- c("ing_inz", "dddinininiging")

str_detect(x, "((ing)|(ize))$")
```

    ## [1]  TRUE FALSE  TRUE

``` r
str_detect(y, "((ing)|(ize))$")
```

    ## [1] FALSE  TRUE

``` r
x <- c("aeed", "seed", "sed", "aed")
str_detect(x, "[^e]ed$") # eed로 끝나지 않고 ed로 끝나는 거 찾아줌
```

    ## [1] FALSE FALSE  TRUE  TRUE

``` r
# c 뒤를 제외하고 i가 e 앞에 온다.
words[str_detect(stringr::words, "cie")]
```

    ## [1] "science" "society"

``` r
words[str_detect(stringr::words, "ei")]
```

    ## [1] "eight"   "either"  "receive" "weigh"

``` r
# q 다음은 항상 u인가?
words[str_detect(words, "q[^u]")]
```

    ## character(0)

``` r
# 전화번호를 매칭하는 정규표현식 작성
x <- c("010-3299-1351", "010-xxxx-1351", "011-3333-5555")
str_detect(x, "^(010)\\-\\d\\d\\d\\d\\-\\d\\d\\d\\d") # 010으로 시작하는 번호만
```

    ## [1]  TRUE FALSE FALSE

## (4) 반복

? : 0 또는 1회 + : 1회 이상 \* : 0회 이상 {n ,m} n회 이상 m회 이하

이러한 반복 매칭을 그리디 매칭이라고 한다.

만들어 놓은 반복 조건에서 가능한 가장 긴 문자열과 매칭시키려고 한다.

뒤에 **?**를 추가하면 주어진 조건에서 가장 짧은 문자열과 매칭된다.

``` r
x <- "1888 is the longest year in roman numerals : MDCCCLXXXVIII"

str_match(x, "CC+")
```

    ##      [,1] 
    ## [1,] "CCC"

``` r
str_match(x, "CC?")
```

    ##      [,1]
    ## [1,] "CC"

``` r
str_match(x, "c[LX]+")
```

    ##      [,1]
    ## [1,] NA

``` r
str_match(x, "c{1}")
```

    ##      [,1]
    ## [1,] NA

``` r
str_match(x, "c{2,}")
```

    ##      [,1]
    ## [1,] NA

``` r
str_match(x, "c{1,3}?")
```

    ##      [,1]
    ## [1,] NA

``` r
str_match(x, "C[LX]+?")
```

    ##      [,1]
    ## [1,] "CL"

: 역참조할 수 있는 그룹을 정의한다고 함.

괄호안에 표현식을 입력하는 것은, 하위 표현식으로 그룹화한다는 뜻이다.

하위 표현식은 차례로 **임시 버퍼**에 저장된다.

사용하면 임시 버퍼에 저장된 표현식들을 번호를 통해서 사용한다는 뜻이다.

``` r
# 두 글자가 반복되는 과일 이름과 매칭하는 정규표현식
# (..) 이게 우리가 만든 하나의 정형화된 표현
fruit <- stringr::fruit
fruit[str_detect(fruit, "(..)\\1")]
```

    ## [1] "banana"      "coconut"     "cucumber"    "jujube"      "papaya"     
    ## [6] "salal berry"

``` r
x <- c("axxx", "abba", "caca", "cacvc", "abcqqwwcba")
str_match(x, "(.)\\1\\1")
```

    ##      [,1]  [,2]
    ## [1,] "xxx" "x" 
    ## [2,] NA    NA  
    ## [3,] NA    NA  
    ## [4,] NA    NA  
    ## [5,] NA    NA

``` r
str_match(x, "(.)(.)\\2\\1")
```

    ##      [,1]   [,2] [,3]
    ## [1,] NA     NA   NA  
    ## [2,] "abba" "a"  "b" 
    ## [3,] NA     NA   NA  
    ## [4,] NA     NA   NA  
    ## [5,] NA     NA   NA

``` r
# 같은 문자로 시작하고 끝나는 단어 매칭
x <- c("abbbccwsda", "aggdfgsdhtsdn")

str_match(x, "^(.).*\\1$")
```

    ##      [,1]         [,2]
    ## [1,] "abbbccwsda" "a" 
    ## [2,] NA           NA

``` r
# 두 문자 반복이 있는 단어 매칭
x <- c("church", "afbdfgd", "fdhiogudhjkuy")
str_match(x, ".*(.{2}).*\\1.*")
```

    ##      [,1]            [,2]
    ## [1,] "church"        "ch"
    ## [2,] NA              NA  
    ## [3,] "fdhiogudhjkuy" "dh"

``` r
# 적어도 세 곳에서 반복되는 문자가 있음
x <- c("eleven", "abcd", "churchurch")
str_match(x, ".*(.+).*\\1.*\\1.*")
```

    ##      [,1]         [,2]
    ## [1,] "eleven"     "e" 
    ## [2,] NA           NA  
    ## [3,] "churchurch" "h"

# 9. 유용한 함수

## (1) 매칭 탐지

str_detect(): 결과가 true/false로 나온다.

``` r
mean(str_detect(words, "^t")) # 활용 예시 1 / t로 시작하는 단어의 비율
```

    ## [1] 0.06632653

``` r
# x로 끝나거나 시작하는 모든 단어
words[str_detect(words, "^x") | str_detect(words, "x$")]
```

    ## [1] "box" "sex" "six" "tax"

str_subset() : 결과가 논리형이 아닌 문자형으로 나온다.

``` r
str_subset(words, "a$")
```

    ## [1] "a"       "america" "area"    "extra"   "idea"    "tea"

``` r
# 영어 알파벳의 모음으로 시작하고 자음으로 끝나는 단어
str_subset(words, "^[aeiou].*[^aeiou]$") %>% head()
```

    ## [1] "about"   "accept"  "account" "across"  "act"     "actual"

str_count() : 하나의 문자열에 몇번 매칭되는지 알려준다.

``` r
str_count(words, "a") %>% head()
```

    ## [1] 1 1 1 1 1 1

## (2) 매칭 추출

str_extract, str_extract_all 함수 활용

``` r
sentences <- stringr::sentences

color <- c("red", "orange", "yellow", "green", "blue", "purple")
color_match <- str_c(color, collapse = "|")

has_color <- str_subset(sentences, color_match)
matches <- str_extract(has_color, color_match)
head(matches)
```

    ## [1] "blue" "blue" "red"  "red"  "red"  "blue"

``` r
str_extract_all(has_color, color_match, simplify = TRUE) %>% head()
```

    ##      [,1]   [,2]
    ## [1,] "blue" ""  
    ## [2,] "blue" ""  
    ## [3,] "red"  ""  
    ## [4,] "red"  ""  
    ## [5,] "red"  ""  
    ## [6,] "blue" ""
