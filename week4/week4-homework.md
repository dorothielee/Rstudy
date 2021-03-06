week4 homework
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

# 함수

함수 생성 3단계

-   함수 이름
-   function 내부에 함수의 인수(args)를 나열한다. function(x, y, z)
-   함수의 본문(body)

함수 이름 설정할 때, 가장 많이 쓰이는 방법은 스네이크 표기법이다.

함수 이름 설정 예시

ExampleFunction : 캐멀 표기법

**example_function** : 스네이크 표기법

example.function : 파이썬에서는 불가능해서 추천하지 않음

가능하면 기본 함수, 변수를 덮어쓰지 않는 것이 좋다.

## (1) 조건부 실행

if문을 사용하여 코드를 조건부 실행한다.

``` r
has_name <- function(x){
  
  nms <-names(x)
  
  if(is.null(nms)){
    
    rep(FALSE, length(x))
    
  } else{
    
    (!is.na(nms)) & (nms !="")
    
  }

}

x <- c(1:5)
has_name(x)
```

    ## [1] FALSE FALSE FALSE FALSE FALSE

``` r
names(x) <- c("1", "2", "3", "4", "5")
has_name(x)
```

    ## [1] TRUE TRUE TRUE TRUE TRUE

## (2) cut 함수를 활용하여 if문 단순화하기

반복하지 말라(Do not repeat yourself, DRY) 원칙

``` r
x <- 1:10

# ?cut
# cut 활용법 : 구간별로 쪼개서 factor로 변환하기 유용함.
cut(x, breaks = 3)
```

    ##  [1] (0.991,4] (0.991,4] (0.991,4] (0.991,4] (4,7]     (4,7]     (4,7]    
    ##  [8] (7,10]    (7,10]    (7,10]   
    ## Levels: (0.991,4] (4,7] (7,10]

``` r
cut(x, breaks = 3, labels = c("1", "2", "3"))
```

    ##  [1] 1 1 1 1 2 2 2 3 3 3
    ## Levels: 1 2 3

``` r
# seq 이용해서 끝 값을 정해줄 수 있음.
cut(x, breaks = seq(from = 1, to = 11, by =2), right = TRUE) # 오른쪽 괄호 닫힌구간 default
```

    ##  [1] <NA>   (1,3]  (1,3]  (3,5]  (3,5]  (5,7]  (5,7]  (7,9]  (7,9]  (9,11]
    ## Levels: (1,3] (3,5] (5,7] (7,9] (9,11]

## (3) 함수의 인수 : 특수 인수

…: 임의 개수의 인수를 가져온다.

``` r
rule <- function(..., pad = "-") {
  
  title <- paste0(...)
  width <- getOption("width") - nchar(title) - 5
  
  cat(title, " ", stringr::str_dup(pad, width), "\n", sep = "")
  
}

rule("ABCDE", "cdfas")
```

    ## ABCDEcdfas -----------------------------------------------------------------

## (4) 반환

명시적 반환문 : return을 사용하여 반환하는 것. 주로 일찍 반환 시킬 때
사용한다.

``` r
complicated_function <- function(x,y,z) {
  
  if (length(x) == 0 | length(y) == 0) {
    
    return(0)
    
  } else {
    
    z <- 10
    
  }
  
  x
  
}

complicated_function(c(1,2,3),c(4,5,6), NA) %>% length()
```

    ## [1] 3

``` r
# 함수형 프로그램
# 필요하면 찾아보기
# purrr::map()
```
