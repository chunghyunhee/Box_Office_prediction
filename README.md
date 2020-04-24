# TMDB Box Office Prediction

## goal 
- predict the revenue of the movie

## data overvies
- Data Description id - Integer unique id of each movie

- belongs_to_collection - Contains the TMDB Id, Name, Movie Poster and Backdrop URL of a movie in JSON format. You can see the Poster and Backdrop Image like this: https://image.tmdb.org/t/p/original/. Example: https://image.tmdb.org/t/p/original//iEhb00TGPucF0b4joM1ieyY026U.jpg

- budget:Budget of a movie in dollars. 0 values mean unknown.

- genres : Contains all the Genres Name & TMDB Id in JSON Format

- homepage - Contains the official homepage URL of a movie. Example: http://sonyclassics.com/whiplash/ , this is the homepage of Whiplash movie.

- imdb_id - IMDB id of a movie (string). You can visit the IMDB Page like this: https://www.imdb.com/title/

- original_language - Two digit code of the original language, in which the movie was made. Like: en = English, fr = french.

- original_title - The original title of a movie. Title & Original title may differ, if the original title is not in English.

- overview - Brief description of the movie.

- popularity - Popularity of the movie in float.

- poster_path - Poster path of a movie. You can see the full image like this: https://image.tmdb.org/t/p/original/

- production_companies - All production company name and TMDB id in JSON format of a movie.

- production_countries - Two digit code and full name of the production company in JSON format.

- release_date - Release date of a movie in mm/dd/yy format.

- runtime - Total runtime of a movie in minutes (Integer).

- spoken_languages - Two digit code and full name of the spoken language.

- status - Is the movie released or rumored?

- tagline - Tagline of a movie

- title - English title of a movie

- Keywords - TMDB Id and name of all the keywords in JSON format.

- cast - All cast TMDB id, name, character name, gender (1 = Female, 2 = Male) in JSON format

- crew - Name, TMDB id, profile path of various kind of crew members job like Director, Writer, Art, Sound etc.

- revenue - Total revenue earned by a movie in dollars.

## procedure
1. 예측하고자 하는 target값 확인(revenue in df_train)

2. 각 feature별 처리
- 필요한 데이터 selection(여기서는 포괄적으로 잡고, 뒤에서 PCA를 통해 잠재변수 생성, feature selection으로 중요한 변수를 골라낼 예정)
- 기타 null값 제거, reindex, stringToLiteral 작업 등
- 개별 col에 대한 작업
    - production countries : 
    1. dict 형태에서 첫번째 국가로 변환
    2. 북미, 유럽, 기타 국가로 카테고리를 지정(지정 기준: value_counts를 통해 자주 나오는 국가로 취합한 결과 )
    - spoken languages
    1. production countries 칼럼과 비슷하나 약간 다름(소문자 사용 등)
    2. 절대다수 영어이기 때문에 영어와 그외로 구분함. 
    - genre : dict 벗기고 인코딩 칼럼변환
    - budget : 영화예산을 자체적으로 3등분하여 등급을 지정함 
    - cast(출연배우) 
    1. 규모로 파악 (출연배우의 수 ~ 규모)
    2. 비싼 영화에 출연햇는지의 여부로 배우의 등급을 생성. 즉 cast의 dict를 벗겨내어 주요 출연자만 5명 뽑히도록 리스트 생성, budget level에 따라 비싼 영화에 출연한 배우 리스트, 중급영화에 출연한 배우 리스트를 만든다, 주요 출연진이 어느 등급에 속했는지에 따라  점수를 매겨 합해 score로 출력
    - 기타 쓸모 없어진 칼럼들 제거 
- 인코딩으로 nan생성값 0으로 변환
- 마지막으로 앞에서 처리한 categorical value들 원핫인코딩으로 변환

3. modeling
- lightGBM : 
- RandomForestRegressor : 
- PCA로 잠재변수 생성, feature importance에서 몇가지 feature selection을 진행함. 

4. evalution & subbission & comment 

