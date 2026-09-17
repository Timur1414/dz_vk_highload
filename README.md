# Проектирование высоконагруженных систем
### (Курсовая работа VK Education)

# Spotify

# Содержание
- [1. Тема и целевая аудитория](#1-тема-и-целевая-аудитория)
  - [1.1. Тема](#11-тема)
  - [1.2. Существующие аналоги](#12-существующие-аналоги)
  - [1.3. Целевая аудитория](#13-целевая-аудитория)
  - [1.4. Функционал](#14-функционал)
- [2. Расчет нагрузки](#2-расчет-нагрузки)
  - [2.1 Продуктовые метрики](#21-продуктовые-метрики)
  - [2.2 Технические метрики](#22-технические-метрики)
- [Источники](#источники)

# 1. Тема и целевая аудитория
## 1.1. Тема
Spotify - музыкальный стриминговый сервис. Компания вышла на биржу в 2018 году. Spotify занимает 34% глобального рынка стриминга (информация на 2021 год). Использует freemium‑модель: предлагает бесплатный доступ с рекламой и премиум‑подписку без рекламы [1]. Прибыль сервиса составляет 721 миллион евро на момент 2 квартала 2026 года, хотя до 2024 года компания терпела убытки. Чтобы выйти из зоны убытков, компания несколько раз повышала цены на подписки. При этом акции Spotify упали более чем на 13% [2].

Spotify достаточно сложная система:
- 700+ миллион активных пользователей в месяц
- 1.5 петабайт хранимых аудиофайлов
- 50 гигабайт в секунду потоковая передача данных
- умные рекомендации
- поиск с задержкой менее 50 миллисекунд
- синхронизация между устройствами
- несколько форм аудиоконтента 
- собственная DNS архитектура [5]
## 1.2. Существующие аналоги
Рынок музыкального стриминга в 2025 году приблизился к 1 млрд. пользователей [3]. При этом в топ входят следующие продукты:
- Spotify - лидер рынка, который занимает долю в 31.4%.
- Tencent Music - китайский сервис, который занимает долю в 13.8%.
- Apple Music - сервис от Apple, который занимает долю в 12.6%.
- YouTube Music - самый быстрорастущий сервис, который пока что занимает долю в 12.4%.
- Amazon Music - замыкает пятёрку лидеров с долей в 8.5%.
- Яндекс Музыка - российский сервис, который занимает примерно 3.4%.

Главным конкурентов для Spotify является YouTube Music [4]. Темпы роста YouTube Music почти в два раза превышают темпы роста Spotify.
## 1.3. Целевая аудитория
Общее количество активных пользователей в месяц составляет 761 млн. по всему миру. Из них число премиум пользователей составляет 293 млн. (информация на первый квартал 2026 года) [2]. Большая часть слушателей находится в Европе - 26% от общего числа месячных активных пользователей. Также регион Европы является самым большим по количеству премиум подписок. Точного значения ежедневной активной аудитории компания не предоставляет, но если оценить, что в день сервисом пользуется примерно 30% от MAU, то ежедневная активная аудитория составляет примерно 230 млн. пользователей. 

В поведении пользователей можно выделить 3 ключевых момента [8]:
- утренний пик (8:00 - 10:00) - этот период связан с поездками на работу или учёбу
- вечерний пик (17:00-23:00) - самое активное время для прослушивания
- ночные часы (примерно после полуночи до 6 утра) - может быть связано с прослушиванием перед сном или в качестве фонового шума
## 1.4. Функционал
Полный функционал сервиса включает в себя:
- потоковое воспроизведение аудио с адаптивным битрейтом
- авторизация и управление профилем
- добавление треков в собственную библиотеку
- бесшовное воспроизведение альбомов
- плавный переход между треками
- поиск по трекам, композиторам, альбомам и плейлистам
- оффлайн режим
- синхронизация между устройствами
- персонализированные рекомендации
- общие плейлисты
- прослушивание подкастов
- отображение синхронизированных текстов песен
- Spotify Connect — управление воспроизведением между устройствами
- AI Playlist — создание плейлистов по текстовому описанию [7]

В MVP будет входить следующий функционал:
- авторизация и управление профилем
- потоковое воспроизведение аудио 
- добавление треков в собственную библиотеку
- поиск по трекам, композиторам, альбомам и плейлистам
- персонализированные рекомендации
- отображение синхронизированных текстов песен
- (диз)лайки треков

# 2. Расчет нагрузки
## 2.1. Продуктовые метрики
|Метрики                               |Значение                                                          |
|--------------------------------------|------------------------------------------------------------------|
|MAU                                   |761 млн.                                                          |
|DAU                                   |230 млн.                                                          |
|Премиум пользователи                  |293 млн.                                                          |
|Время прослушивания (на пользователя) |148 мин/день [9]                                                  |
|Количество треков                     |100 млн. [18]                                                     |
|Объём аудио (без учёта кодировок)     |300 ТБ [10]                                                       |
|Потребление данных (битрейт)          |Обычное качество: 96 kbps [11]<br>Высокое качество: 160 kbps [11] |
|Прослушивание треков                  |11 млрд. в день [12]                                              |
|Количество операций лайков            |211 млн. в день                                                   |
|Количество поисковых запросов         |690 млн. в день                                                   |

В spotify трек считается прослушанным, если он играет более 30 секунд.

Точной информации по лайкам у spotify нет, поэтому была использована информация из открытых данных Яндекс Музыки [19]. 
По информации Яндекс Музыки пользователи ставят 1 лайк на 52 прослушивания => количество операций лайков = DAU * 0,0192 = 211 млн. в день.

Точной информации по операциям поиска у spotify нет, поэтому было выбрано значение, что пользователь в среднем совершает 3 поиска в день [6] => 
количество поисковых запросов = DAU * 3 = 690 млн. в день.
## 2.2. Технические метрики
|Данные                                |Размер                                      |
|--------------------------------------|--------------------------------------------|
|Объём аудио (с учётом всех кодировок) |1 ПБ. [14]                                  |
|Метаданные                            |500 ТБ. [14]                                |
|Плейлисты                             |500 ТБ. [14]                                |
|Обложки треков                        |30 ТБ. [14]                                 |
|Задержка воспроизведения нового трека |200 мс. [15]                                |
|Задержка обработки события паузы      |5 мс. [!16]                                  |
|Задержка API                          |50 мс. [15]                                 |
|API запросы (всего)                   |11 млн. RPS [17]                            |
|Общая пропускная способность          |50 ГБ/с                                     |
|Прослушивания RPS                     |Средний: 145000 RPS<br>Пиковый: 450000 RPS  |
|Поиск RPS                             |Средний: 10000 RPS<br>Пиковый: 40000 RPS    |
|Загрузка плейлиста RPS                |Средний: 25000 RPS<br>Пиковый: 100000 RPS   |
|Лайки RPS                             |Средний: 2500 RPS<br>Пиковый: 7500 RPS      |
|Рекомендации RPS                      |Средний: 15000 RPS<br>Пиковый: 50000 RPS    |
|Авторизация RPS                       |Средний: 500 RPS<br>Пиковый: 5000 RPS       |
|Аудио стриминг RPS                    |Средний: 580000 RPS<br>Пиковый: 2000000 RPS |
|Кодеки                                |Ogg Vorbis и AAC                            |

Лайки RPS = 211 млн. / 86400 = 2500

Так как точных данных пиковых значений нет, то выбран коэффициент пика = 3.
Пиковый RPS = средний RPS * 3
# Источники
1. Анализ Spotify (SPOT). URL: [https://longterminvestments.ru/spotify-analysis?ysclid=mtsg3ro0nb502972528](https://longterminvestments.ru/spotify-analysis?ysclid=mtsg3ro0nb502972528)
2. Spotify has 293 million premium subscribers. URL: [https://www.heise.de/en/news/Spotify-has-293-million-premium-subscribers-11275204.html?wt_mc=sm.red.ho.mastodon.mastodon.md_beitraege.md_beitraege&utm_source=mastodon#1](https://www.heise.de/en/news/Spotify-has-293-million-premium-subscribers-11275204.html?wt_mc=sm.red.ho.mastodon.mastodon.md_beitraege.md_beitraege&utm_source=mastodon#1)
3. The music industry is closing in on a billion global subscribers – with Spotify out in front. URL: [https://www.musicbusinessworldwide.com/the-music-industry-is-closing-in-on-a-billion-global-subscribers-with-spotify-out-in-front/#1](https://www.musicbusinessworldwide.com/the-music-industry-is-closing-in-on-a-billion-global-subscribers-with-spotify-out-in-front/#1)
4. Music subscriber market shares Q4 2025: The chess board is set. URL: [https://www.midiaresearch.com/blog/music-subscriber-market-shares-q4-2025-the-chess-board-is-set](https://www.midiaresearch.com/blog/music-subscriber-market-shares-q4-2025-the-chess-board-is-set)
5. Spotify’s Love/Hate Relationship with DNS. URL: [https://engineering.atspotify.com/2017/03/spotifys-love-hate-relationship-with-dns](https://engineering.atspotify.com/2017/03/spotifys-love-hate-relationship-with-dns)
6. Rethinking Spotify Search. URL: [https://engineering.atspotify.com/2021/4/rethinking-spotify-search?utm_source=chatgpt.com](https://engineering.atspotify.com/2021/4/rethinking-spotify-search?utm_source=chatgpt.com)
7. 25 Ways Spotify Leveled Up Your Listening in 2025. URL: [https://newsroom.spotify.com/2025-12-29/year-in-features/](https://newsroom.spotify.com/2025-12-29/year-in-features/)
8. Spotify reveals when India tunes in: Gen Z and millennials shape daily music rituals. URL: [https://www.businesstoday.in/technology/news/story/spotify-reveals-when-india-tunes-in-gen-z-and-millennials-shape-daily-music-rituals-482131-2025-06-27?referral=yes&t_content=footerstrip-1&t_medium=web&t_psl=False&t_source=recengine#1](https://www.businesstoday.in/technology/news/story/spotify-reveals-when-india-tunes-in-gen-z-and-millennials-shape-daily-music-rituals-482131-2025-06-27?referral=yes&t_content=footerstrip-1&t_medium=web&t_psl=False&t_source=recengine#1)
9. Spotify Statistics UK: 15.3M Subscribers, First Profit. URL: [https://songgifts.co.uk/blog/spotify-statistics/#sources_18](https://songgifts.co.uk/blog/spotify-statistics/#sources_18)
10. Anna’s Archive releases massive 300TB Spotify music scrape. URL: [https://cyberinsider.com/annas-archive-releases-massive-300tb-spotify-music-scrape/#genesis-content#1](https://cyberinsider.com/annas-archive-releases-massive-300tb-spotify-music-scrape/#genesis-content#1)
11. Audio quality. URL: [https://support.spotify.com/us/article/audio-quality/?utm_source=chatgpt.com]([https://www.giga.de/tech/spotify-wie-hoch-ist-der-datenverbrauch--01J5QMXR2CW20T1F4R77CARQRN#doc-W7IJGiCPoL#1](https://support.spotify.com/us/article/audio-quality/?utm_source=chatgpt.com)
12. What 20 Years of Spotify Data Reveals About Our Listeners. URL: [https://newsroom.spotify.com/2026-04-23/spotify-20-data-listening-trends/?utm_source=chatgpt.com](https://newsroom.spotify.com/2026-04-23/spotify-20-data-listening-trends/?utm_source=chatgpt.com)
13. . URL: []()
14. Introducing cstar: The Spotify Cassandra orchestration tool, now open source. URL: [https://engineering.atspotify.com/2018/9/introducing-cstar-the-spotify-cassandra-orchestration-tool-now-open-source](https://engineering.atspotify.com/2018/9/introducing-cstar-the-spotify-cassandra-orchestration-tool-now-open-source)
15. Designing Spotify. URL: [https://medium.com/@tejasd603/designing-spotify-a-10-minute-deep-dive-into-scalable-audio-streaming-architecture-8dec26d05a37#1](https://medium.com/@tejasd603/designing-spotify-a-10-minute-deep-dive-into-scalable-audio-streaming-architecture-8dec26d05a37#1)
16. . URL: []()
17. Spotify's Niklas Gustavsson on Scalability and Engineering Excellence. URL: [https://www.linkedin.com/posts/byndcode_interview-with-spotifys-chief-architect-activity-7441422475495698432-Y0Gu#1](https://www.linkedin.com/posts/byndcode_interview-with-spotifys-chief-architect-activity-7441422475495698432-Y0Gu#1)
18. About Spotify. URL: [https://investors.spotify.com/about/?utm_source=chatgpt.com](https://investors.spotify.com/about/?utm_source=chatgpt.com)
19. Yandex  Music Billion-Interactions Dataset. URL: [https://ya.ru/ai/papers/yandex-music-billion-interactions-dataset](https://ya.ru/ai/papers/yandex-music-billion-interactions-dataset)
