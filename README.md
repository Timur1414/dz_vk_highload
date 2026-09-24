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
- [3. Глобальная балансировка нагрузки](#3-глобальная-балансировка-нагрузки)
  - [3.1. Разбиение по доменам](#31-разбиение-по-доменам)
  - [3.2. Расположение дата-центров](#32-расположение-дата-центров)
  - [3.3. Схема DNS](#33-схема-dns)
  - [3.4. Схема Anycast](#34-схема-anycast)
  - [3.5. Регулирование трафика между дата-центрами](#35-регулирование-трафика-между-дата-центрами)
- [Источники](#источники)

# 1. Тема и целевая аудитория
## 1.1. Тема
Spotify - музыкальный стриминговый сервис. Компания вышла на биржу в 2018 году. Spotify занимает 34% глобального рынка стриминга (информация на 2021 год).
Использует freemium‑модель: предлагает бесплатный доступ с рекламой и премиум‑подписку без рекламы [1]. Прибыль сервиса составляет 721 миллион евро на момент 2 квартала 2026 года, 
хотя до 2024 года компания терпела убытки. Чтобы выйти из зоны убытков, компания несколько раз повышала цены на подписки. При этом акции Spotify упали более чем на 13% [2].

Spotify достаточно сложная система:
- 700+ миллион активных пользователей в месяц
- 1.5 петабайт хранимых аудиофайлов
- 50 гигабайт в секунду потоковая передача данных
- умные рекомендации
- поиск с задержкой менее 50 миллисекунд
- синхронизация между устройствами
- несколько форм аудио контента 
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
Общее количество активных пользователей в месяц составляет 761 млн. по всему миру. Из них число премиум пользователей составляет 293 млн. (информация на первый квартал 2026 года) [2].
Большая часть слушателей находится в Европе - 26% от общего числа месячных активных пользователей. Также регион Европы является самым большим по количеству премиум подписок.
Точного значения ежедневной активной аудитории компания не предоставляет, но если оценить, что в день сервисом пользуется примерно 30% от MAU, то ежедневная активная аудитория составляет 
примерно 230 млн. пользователей. 

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
- офлайн режим
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
| Метрики                               | Значение                                                          |
|---------------------------------------|-------------------------------------------------------------------|
| MAU                                   | 761 млн.                                                          |
| DAU                                   | 230 млн.                                                          |
| Премиум пользователи                  | 293 млн.                                                          |
| Время прослушивания (на пользователя) | 148 мин/день [9]                                                  |
| Количество треков                     | 100 млн. [18]                                                     |
| Объём аудио (без учёта кодировок)     | 300 ТБ [10]                                                       |
| Потребление данных (битрейт)          | Обычное качество: 96 kbps [11]<br>Высокое качество: 160 kbps [11] |
| Прослушивание треков                  | 11 млрд. в день [12]                                              |
| Количество операций лайков            | 211 млн. в день                                                   |
| Количество поисковых запросов         | 690 млн. в день                                                   |

В Spotify трек считается прослушанным, если он играет более 30 секунд.

Точной информации по лайкам у Spotify нет, поэтому была использована информация из открытых данных Яндекс Музыки [19]. 
По информации Яндекс Музыки пользователи ставят 1 лайк на 52 прослушивания ⇒ количество операций лайков = DAU * 0,0192 = 211 млн. в день.

Точной информации по операциям поиска у Spotify нет, поэтому было выбрано значение, что пользователь в среднем совершает 3 поиска в день [6] ⇒ 
количество поисковых запросов = DAU * 3 = 690 млн. в день.
## 2.2. Технические метрики
| Данные                                | Размер           |
|---------------------------------------|------------------|
| Объём аудио (с учётом всех кодировок) | 1 ПБ. [14]       |
| Метаданные                            | 500 ТБ. [14]     |
| Плейлисты                             | 500 ТБ. [14]     |
| Обложки треков                        | 30 ТБ. [14]      |
| Задержка воспроизведения нового трека | 200 мс. [18]     |
| Задержка обработки события паузы      | 5 мс. [18]       |
| Задержка API                          | 50 мс. [18]      |
| API запросы (всего)                   | 11 млн. RPS [17] |
| Общая пропускная способность          | 50 ГБ/с [18]     |
| Аудио стриминг (CDN)                  | 2.26 Тб/с        |
| Кодеки                                | Ogg Vorbis и AAC |

Под аудио стримингом подразумевается раздача аудиофайлов через CDN. Данные отдаются чанками по 512 КБ [15]. 
Количество пользователей, слушающих одновременно = DAU * 148 * 60 / 86400 = 23.6 млн.
Пропускная способность при битрейте 96 kbps = 23.6 млн. * 96 = 2.26 Тб/с

| RPS           | Средний | Пиковый |
|---------------|---------|---------|
| Прослушивания | 1000000 | 3000000 |
| Поиск         | 8000    | 24000   |
| Лайки         | 2500    | 7500    |
| Рекомендации  | 13000   | 39000   |
| Авторизация   | 500     | 1500    |

В среднем на прослушивание трека (длиной 3 минуты) возникает 8 API вызовов: 1 - старт трека, 1 - конец трека, каждые 30 секунд - пинг для статистики [13].
Тогда RPS прослушивания = 11 млрд. * 8 / 86400 = 1 млн.

Лайки RPS = 211 млн. / 86400 = 2500.

Система рекомендаций в Spotify разделена на 2 контура: мл занимается созданием плейлистов (миксы, радары новинок и т.д.) и сохраняет их в кэш;
при просмотре домашней страницы сервис берёт предрассчитанные плейлисты из кэша [16].
Каждый пользователь открывает домашнюю страницу примерно 5 раз в день [20] ⇒ RPS рекомендаций (учитываются только обращения к предрассчитанному кэшу) = 
DAU * 5 / 86400 = 13000.

Поиск RPS - запросы от пользователя, которые ищут в бд треки по названию, артисту или плейлисту = 690 млн. / 86400 = 8000

Так как точных данных пиковых значений нет, то выбран коэффициент пика = 3.
Пиковый RPS = средний RPS * 3

# 3. Глобальная балансировка нагрузки
В архитектуре Spotify используется четкое разделение между клиентским периметром и внутренней инфраструктурой сети [5]:
- Зона .net — внутренняя инфраструктура.
- Зона .com — внешние публичные интерфейсы, доступные клиентам сервиса.

## 3.1. Разбиение по доменам
Публичные функциональные домены:
- accounts.spotify.com — авторизация, регистрация, OAuth 2.0 / OpenID Connect, сессии пользователей, биллинг и управление подписками. Выделен в изолированный домен для безопасности.
- api.spotify.com — публичный RESTful Web API для внешних интеграций, сторонних разработчиков, интеграций с умными устройствами и веб-плеера.
- spclient.wg.spotify.com — основной API-шлюз для нативных клиентов (iOS, Android, Desktop, macOS). Обслуживает домашнюю страницу, рекомендации, плейлисты, медиатеку,
синхронизированные тексты песен, социальные функции и отправку телеметрии. + выбор оптимального CDN.
- apresolve.spotify.com / ap.spotify.com — Access Point Resolver. Специализированный шлюз разрешения точек входа для прямого управляющего мультиплексированного протокола Spotify,
через который клиент получает метаданные стрима и команды управления воспроизведением (Spotify Connect). Возвращает клиенту гео-оптимизированный список IP шлюзов доступа.
- audio-*.spotifycdn.com — доставка аудио контента (CDN). Отдельный домен изолирует тяжелый трафик от легкого API-трафика, оптимизирует отдачу чанков по 512 КБ.
- i.scdn.co / image-*.spotifycdn.com — статический контент и изображения. Раздается через гео-распределенный CDN с агрессивным заголовком кэширования (Cache-Control: public, max-age=31536000, immutable).
- open.spotify.com — веб-клиент (SPA).

## 3.2. Расположение дата-центров
Исторически Spotify имела 4 основных вычислительных кластера дата-центров, в которых расположены бд, сервисы ML-рекомендаций, поиска, биллинга. Они были расположены в 
Лондоне, Стокгольме, Эшберне и Сан-Хосе [21].

В 2018 году компания мигрировала на Google Cloud Platform [22]. После этого Spotify имеет дата-центры в 5 регионах [23]:
- europe-west1 (Сен-Гислен, Бельгия)
- us-central1 (Каунсил-Блафс, Айова)
- asia-east1 (Тайвань)
- us-east1 (Монкс-Корнер, Южная Каролина)
- europe-west4 (Эмсхавен, Нидерланды)

Spotify имеет по несколько кластеров в Европе и Америке по причине, что эти регионы образуют большую часть MAU и премиум подписчиков всего сервиса.
Кластер в Тайване нужен так как Азиатско-Тихоокеанический регион является самым быстрорастущим сегментом для Spotify.

RTT до ключевых европейских столиц составляет 5-15 мс.
RTT до Нью-Йорка, Вашингтона, Торонто и Майами не превышает 20 мс.
RTT в любую точку континентальной части США не превышает 30 мс.
RTT до ключевых азиатских мегаполисов составляет 20-40 мс.

Крупные IXP рядом с кластерами:
- AMS-IX расположен в 200 км. от europe-west4. RTT составляет 3 мс. Также из europe-west4 выходят трансатлантические кабели.
- BN-IX расположен в 70 км. от europe-west1. RTT составляет 1 мс.
- Atlanta-IX расположен в 450 км. от us-east1. RTT составляет 7 мс. Также рядом находится Ашберн, который является крупнейшим дата-центровым кластером.
- KC-IX расположен в 290 км. от us-central1. RTT составляет 4 мс. Этот кластер оптимизирован под ML вычисления, что необходимо для рекомендаций.
- TW-IX расположен в 179 км. от asia-east1. RTT составляет 2 мс. Также из asia-east1 выходят тихоокеанские кабели.

На территории США 4 часовых пояса. Наличие двух дата-центров позволяет распределять нагрузку по мере того, как волна вечернего пика смещается с востока на запад страны.

Расстояние между Сен-Гисленом (Бельгия) и Эмсхавеном (Нидерланды) — около 300 км. Это дает сетевую задержку между самими дата-центрами 3-4 мс.
Это позволяет базам данных реплицироваться с низкой задержкой, но при этом дата-центры находятся в разных энергосетях и исключают общую точку отказа.

Если бы запрос шёл через океан, то RTT уже было бы 100-150 мс. + накладные расходы на TLS и сетевые задержки уже превышают 200 мс. 
Если расположить дата-центры в ключевых регионах, то RTT будет 15-25 мс. В этом случае поход в API и загрузка первого чанка трека укладывается в 200 мс. Аналогично для поиска.
Региональные дата-центры и точки CDN минимизируют джиттер и потерю пакетов при переключении между сотами в мобильных сетях. 

Расположение локальных дата-центров позволяет выполнять юридические требования разных стран (GDPR).

Айова обладает огромными мощностями и дешевой энергией, поэтому именно здесь развернуты кластеры обучения рекомендательных моделей Spotify.

Spotify не создавала собственную CDN сеть, а закупила мощности у нескольких CDN-провайдеров:
- Akamai - доставка аудио (audio-ak.spotify.com).
- Fastly - кэширование API-ответов, метаданных и аудио (audio-fa.spotify.com).
- CloudFlare - защита от DDoS, Anycast-маршрутизация и раздача статики веб плеера (open.spotify.com).
- Google Cloud CDN - нативный CDN от Google Cloud.

Причины отказа от собственного CDN:
- Если у CDN провайдера случается глобальный сбой, то бэкенд Spotify быстро переключает генерацию ссылок для клиентов на резервный CDN (spclient.wg.spotify.com).
- Разные CDN провайдеры лучше работают в разных регионах.
- Экономическая неэффективность.

Распределение запросов по дата-центрам:

| Дата-центр                   | Процент аудитории |
|------------------------------|-------------------|
| europe-west1<br>europe-west4 | 28%               |
| us-central1                  | 24%               |
| us-east1                     | 25%               |
| asia-east1                   | 23%               |

Запросы, связанные с биллингом, регистрацией и загрузкой новых треков выполняются только на кластерах в Европе и США. 

| Тип запроса / Нагрузка         | Суммарно           | europe-west1 (14%) | europe-west4 (14%) | us-east1 (25%)    | us-central1 (24%) | asia-east1 (23%)  |                                                                                     
|--------------------------------|--------------------|--------------------|--------------------|-------------------|-------------------|-------------------|                                                                                                                                                                          
| API Прослушивания (сред./пик)  | 1000000 / 3000000  | 140000 / 420000    | 140000 / 420000    | 250000 / 750000   | 240000 / 720000   | 230000 / 690000   |                                                                                           
| Поиск (сред./пик)              | 8000 / 24000       | 1120 / 3360        | 1120 / 3360        | 2000 / 6000       | 1920 / 5760       | 1840 / 5520       |                                                                                              
| Рекомендации (сред./пик)       | 13000 / 39000      | 1820 / 5460        | 1820 / 5460        | 3250 / 9750       | 3120 / 9360       | 2990 / 8970       |                                                                                      
| Лайки (сред./пик)              | 2500 / 7500        | 350 / 1050         | 350 / 1050         | 625 / 1875        | 600 / 1800        | 575 / 1725        |                                                                                                         
| Авторизация (сред./пик)        | 500 / 1500         | 70 / 210           | 70 / 210           | 125 / 375         | 120 / 360         | 115 / 345         |                                                                                                                 
| Стриминг аудио CDN (сред./пик) | 2.26 / 6.78 Тбит/с | 316 / 949 Гбит/с   | 316 / 949 Гбит/с   | 565 / 1695 Гбит/с | 542 / 1627 Гбит/с | 520 / 1560 Гбит/с |

## 3.3. Схема DNS
В Spotify архитектура DNS разделена на внешний контур (для клиентов) и внутренний контур (инфраструктурный DNS) [5].

Внешний DNS:
GeoDNS, определяющий геопозицию пользователя по префиксу подсети (ECS). Благодаря расширению ESC повышается точность определения пользователя. TTL для API-доменов 60 сек. TTL для статики 24 часа.

Внутренний DNS:
Внутренний DNS имеет несколько уровней.
- Stealth Primary - сервер, который не обслуживает клиентов. Он нужен для создания файлов зон из репозитория. Сами записи генерируются каждые 10 минут.
- Authoritative Secondaries - сервера, которые получают файлы зон от Stealth Primary. Расположены минимум по 2 в каждом географическом регионе. 4 из них доступны из любой точки Интернета.
- Unbound - на каждом сервере запущен Unbound resolver. Это снижает сетевые задержки при поиске адресов сервисов практически до нуля (< 1 мс) и защищает авторитетные DNS-серверы от self-DDoS.
- SRV - Spotify использовали не только A-записи, но и SRV, чтобы микросервисы могли искать друг друга.

Client Error Reporting через DNS.

Когда у мобильного клиента сломан или заблокирован HTTP/HTTPS, он может отправить отчет об ошибке в виде DNS запроса к специальному поддомену.

После миграции на Google Cloud Platform, часть DNS структуры тоже переехала в облако Google. 

## 3.4. Схема Anycast
Spotify полностью отдали Anycast балансировку своим CDN провайдерам.
Для медиа - Anycast на стороне Multi-CDN.
Для API - Anycast на стороне Google.

## 3.5. Регулирование трафика между дата-центрами
Нативные приложения Spotify при старте сначала обращаются к микросервису apresolve.spotify.com, чтобы получить список доступных точек доступа с их весами.
При плановом выводе дата-центра из эксплуатации или аварии веса меняются централизованно (кэш игнорируется). Аналогично и для Multi-CDN.

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
13. Artists deserve transparency about how music streaming works. URL: [https://loudandclear.byspotify.com/](https://loudandclear.byspotify.com/)
14. Introducing cstar: The Spotify Cassandra orchestration tool, now open source. URL: [https://engineering.atspotify.com/2018/9/introducing-cstar-the-spotify-cassandra-orchestration-tool-now-open-source](https://engineering.atspotify.com/2018/9/introducing-cstar-the-spotify-cassandra-orchestration-tool-now-open-source)
15. Smoother Streaming with BBR. URL: [https://engineering.atspotify.com/2018/08/smoother-streaming-with-bbr](https://engineering.atspotify.com/2018/08/smoother-streaming-with-bbr)
16. The Rise (and Lessons Learned) of ML Models to Personalize Content on Home (Part I). URL: [https://stage.engineering.atspotify.com/2021/11/the-rise-and-lessons-learned-of-ml-models-to-personalize-content-on-home-part-i?utm_source=chatgpt.com](https://stage.engineering.atspotify.com/2021/11/the-rise-and-lessons-learned-of-ml-models-to-personalize-content-on-home-part-i?utm_source=chatgpt.com)
17. Spotify's Niklas Gustavsson on Scalability and Engineering Excellence. URL: [https://www.linkedin.com/posts/byndcode_interview-with-spotifys-chief-architect-activity-7441422475495698432-Y0Gu#1](https://www.linkedin.com/posts/byndcode_interview-with-spotifys-chief-architect-activity-7441422475495698432-Y0Gu#1)
18. About Spotify. URL: [https://investors.spotify.com/about/?utm_source=chatgpt.com](https://investors.spotify.com/about/?utm_source=chatgpt.com)
19. Yandex  Music Billion-Interactions Dataset. URL: [https://ya.ru/ai/papers/yandex-music-billion-interactions-dataset](https://ya.ru/ai/papers/yandex-music-billion-interactions-dataset)
20. Understanding User Behavior in Spotify. URL: [https://www.researchgate.net/publication/261060359_Understanding_User_Behavior_in_Spotify]([https://hugoribeiro.com.br/biblioteca-digital/Zhung-Understanding_User_Behavior_in_Spotify.pdf](https://www.researchgate.net/publication/261060359_Understanding_User_Behavior_in_Spotify))
21. SDN Internet Router – Part 2. URL: [https://engineering.atspotify.com/2016/1/sdn-internet-router-part-2?utm_source=chatgpt.com](https://engineering.atspotify.com/2016/1/sdn-internet-router-part-2?utm_source=chatgpt.com)
22. Views From The Cloud: A History of Spotify’s Journey to the Cloud, Part 1. URL: [https://engineering.atspotify.com/2019/12/views-from-the-cloud-a-history-of-spotifys-journey-to-the-cloud-part-1-2?utm_source=chatgpt.com](https://engineering.atspotify.com/2019/12/views-from-the-cloud-a-history-of-spotifys-journey-to-the-cloud-part-1-2?utm_source=chatgpt.com)
23. Fleet Management at Spotify (Part 2): The Path to Declarative Infrastructure. URL: [https://engineering.atspotify.com/2023/05/fleet-management-at-spotify-part-2-the-path-to-declarative-infrastructure?utm_source=chatgpt.com](https://engineering.atspotify.com/2023/05/fleet-management-at-spotify-part-2-the-path-to-declarative-infrastructure?utm_source=chatgpt.com)
24. How Spotify Aligned CDN Services for a Lightning Fast Streaming Experience. URL: [https://engineering.atspotify.com/2020/2/how-spotify-aligned-cdn-services-for-a-lightning-fast-streaming-experience?utm_source=chatgpt.com](https://engineering.atspotify.com/2020/2/how-spotify-aligned-cdn-services-for-a-lightning-fast-streaming-experience?utm_source=chatgpt.com)
