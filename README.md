# Проектирование высоконагруженных систем
### (Курсовая работа VK Education)

# Spotify

# Содержание
- [1. Тема и целевая аудитория](#1-тема-и-целевая-аудитория)
  - [1.1. Тема](#11-тема)
  - [1.2. Существующие аналоги](#12-существующие-аналоги)
  - [1.3. Целевая аудитория](#13-целевая-аудитория)
  - [1.4. Функционал](#14-функционал)
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
Общее количество активных пользователей в месяц составляет 761 млн. по всему миру. Из них число премиум пользователей составляет 293 млн. (информация на первый квартал 2026 года) [2]. Большая часть слушателей находится в Европе - 26% от общего числа месячных активных пользователей. Также регион Европы является самым большим по количеству премиум подписок. Точного значения ежедневной активной аудитории компания не предоставляет, но если оценить, что в день сервисом пользуется примерно 20-30% от MAU, то ежедневная активная аудитория составляет примерно 230 млн. пользователей [6]. 

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
# Источники
1. Анализ Spotify (SPOT). URL: [https://longterminvestments.ru/spotify-analysis?ysclid=mtsg3ro0nb502972528](https://longterminvestments.ru/spotify-analysis?ysclid=mtsg3ro0nb502972528)
2. Spotify has 293 million premium subscribers. URL: [https://www.heise.de/en/news/Spotify-has-293-million-premium-subscribers-11275204.html?wt_mc=sm.red.ho.mastodon.mastodon.md_beitraege.md_beitraege&utm_source=mastodon#1](https://www.heise.de/en/news/Spotify-has-293-million-premium-subscribers-11275204.html?wt_mc=sm.red.ho.mastodon.mastodon.md_beitraege.md_beitraege&utm_source=mastodon#1)
3. The music industry is closing in on a billion global subscribers – with Spotify out in front. URL: [https://www.musicbusinessworldwide.com/the-music-industry-is-closing-in-on-a-billion-global-subscribers-with-spotify-out-in-front/#1](https://www.musicbusinessworldwide.com/the-music-industry-is-closing-in-on-a-billion-global-subscribers-with-spotify-out-in-front/#1)
4. Music subscriber market shares Q4 2025: The chess board is set. URL: [https://www.midiaresearch.com/blog/music-subscriber-market-shares-q4-2025-the-chess-board-is-set](https://www.midiaresearch.com/blog/music-subscriber-market-shares-q4-2025-the-chess-board-is-set)
5. Spotify’s Love/Hate Relationship with DNS. URL: [https://engineering.atspotify.com/2017/03/spotifys-love-hate-relationship-with-dns](https://engineering.atspotify.com/2017/03/spotifys-love-hate-relationship-with-dns)
6. Daily Active Users - Approximation. URL: [https://nitinkc.github.io/system%20design/DAU/](https://nitinkc.github.io/system%20design/DAU/)
7. 25 Ways Spotify Leveled Up Your Listening in 2025. URL: [https://newsroom.spotify.com/2025-12-29/year-in-features/](https://newsroom.spotify.com/2025-12-29/year-in-features/)
8. Spotify reveals when India tunes in: Gen Z and millennials shape daily music rituals. URL: [https://www.businesstoday.in/technology/news/story/spotify-reveals-when-india-tunes-in-gen-z-and-millennials-shape-daily-music-rituals-482131-2025-06-27?referral=yes&t_content=footerstrip-1&t_medium=web&t_psl=False&t_source=recengine#1](https://www.businesstoday.in/technology/news/story/spotify-reveals-when-india-tunes-in-gen-z-and-millennials-shape-daily-music-rituals-482131-2025-06-27?referral=yes&t_content=footerstrip-1&t_medium=web&t_psl=False&t_source=recengine#1)
