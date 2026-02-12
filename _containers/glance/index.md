---
layout: default
title: glance
---

# glance

[GitHub Repository](https://github.com/glanceapp/glance)

Glance is a self-hosted dashboard that puts all your feeds in one place. It's a lightweight, easy-to-use, and highly customizable dashboard that allows you to monitor your services, view RSS feeds, check the weather, and much more.

## Docker Compose Configuration

This is a sample docker-compose file for the `glance` service:

```yaml
services:
  glance:
    container_name: glance
    image: glanceapp/glance
    restart: unless-stopped
    volumes:
      - /root/config/home.yml:/app/config/glance.yml
    ports:
      - 9080:8080
    environment:
      - UPTIME_KUMA_URL={YOUR_UPTIME_KUMA_URL}
      - UPTIME_KUMA_STATUS_SLUG={YOUR_UPTIME_KUMA_STATUS_SLUG}
      - FRED_API_KEY={YOUR_FRED_API_KEY}
```

Here is my current `home.yaml` file config:

NOTE: You will need to load values for the previous env vars for some of the community widgets to work

```
pages:
  - name: Home
    hide-desktop-navigation: true
    columns:
      - size: small
        widgets:
          - type: calendar
            first-day-of-week: monday

          - type: custom-api
            title: LeetCode Daily Question
            cache: 6h
            url: https://leetcode.com/graphql
            method: POST
            headers:
              Accept: application/json
            body-type: json
            body:
              query: |
                query questionOfToday {
                  activeDailyCodingChallengeQuestion {
                    link
                    question {
                      questionId
                      title
                      difficulty
                      topicTags {
                        name
                        slug
                      }
                    }
                  }
                }
              operationName: questionOfToday
            template: |
              <div class="leetcode-card">
                <style>
                  .leetcode-card {
                    max-width: 600px;
                    margin: 8px auto;
                    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
                    transition: transform 0.2s ease, box-shadow 0.2s ease;
                  }
                  .leetcode-card h1 {
                    font-size: 24px;
                    margin: 0 0 16px;
                    line-height: 1.3;
                  }
                  .leetcode-card h1 a {
                    color: #8CAAEE;
                    text-decoration: none;
                    transition: color 0.2s ease;
                  }
                  .leetcode-card h1 a:hover {
                    color: #BAC8FF;
                    text-decoration: underline;
                  }
                  .leetcode-card p {
                    margin: 8px 0;
                    color: #C6D0F5;
                    font-size: 16px;
                  }
                  .leetcode-card .difficulty {
                    font-weight: 600;
                    color: #A5ADCE;
                  }
                  .leetcode-card .difficulty.Easy {
                    color: #A6D189;
                  }
                  .leetcode-card .difficulty.Medium {
                    color: #E5C890;
                  }
                  .leetcode-card .difficulty.Hard {
                    color: #E78284;
                  }
                  .leetcode-card .topics {
                    display: flex;
                    flex-wrap: wrap;
                    gap: 8px;
                    margin: 12px 0;
                  }
                  .leetcode-card .topic-tag {
                    background: #CA9EE6;
                    color: #303446;
                    padding: 4px 12px;
                    border-radius: 16px;
                    font-size: 14px;
                    font-weight: 500;
                    transition: transform 0.2s ease;
                  }
                  .leetcode-card .topic-tag:hover {
                    transform: scale(1.05);
                  }
                  .leetcode-card .premium {
                    color: #E78284;
                    font-weight: 600;
                    margin-top: 12px;
                  }
                  @media (max-width: 600px) {
                    .leetcode-card {
                      padding: 16px;
                      margin: 8px;
                    }
                    .leetcode-card h1 {
                      font-size: 20px;
                    }
                    .leetcode-card p {
                      font-size: 14px;
                    }
                    .leetcode-card .topic-tag {
                      font-size: 12px;
                      padding: 3px 10px;
                    }
                  }
                </style>
                <h1>
                  <a href="https://leetcode.com{{ .JSON.String "data.activeDailyCodingChallengeQuestion.link" }}" target="_blank">
                    {{ .JSON.String "data.activeDailyCodingChallengeQuestion.question.questionId" }} - {{ .JSON.String "data.activeDailyCodingChallengeQuestion.question.title" }}
                  </a>
                </h1>
                <p class="difficulty {{ .JSON.String "data.activeDailyCodingChallengeQuestion.question.difficulty" }}"><strong>Difficulty:</strong> {{ .JSON.String "data.activeDailyCodingChallengeQuestion.question.difficulty" }}</p>
                <p><strong>Topics:</strong></p>
                <div class="topics">
                  {{ if .JSON.Exists "data.activeDailyCodingChallengeQuestion.question.topicTags" }}
                    {{ range .JSON.Array "data.activeDailyCodingChallengeQuestion.question.topicTags" }}
                      <span class="topic-tag">{{ .String "name" }}</span>
                    {{ end }}
                  {{ else }}
                    <span class="topic-tag">None</span>
                  {{ end }}
                </div>
                {{ if .JSON.Bool "data.activeDailyCodingChallengeQuestion.question.paidOnly" }}
                  <p class="premium">This is a Premium question</p>
                {{ end }}
              </div>

          # uptime kuma
          - type: custom-api
            title: Uptime Kuma
            title-url: ${UPTIME_KUMA_URL}
            url: ${UPTIME_KUMA_URL}/api/status-page/${UPTIME_KUMA_STATUS_SLUG}
            subrequests:
              heartbeats:
                url: ${UPTIME_KUMA_URL}/api/status-page/heartbeat/${UPTIME_KUMA_STATUS_SLUG}
            cache: 10m
            template: |
              {{ $hb := .Subrequest "heartbeats" }}

              {{ if not (.JSON.Exists "publicGroupList") }}
              <p class="color-negative">Error reading response</p>
              {{ else if eq (len (.JSON.Array "publicGroupList")) 0 }}
              <p>No monitors found</p>
              {{ else }}

              <ul class="dynamic-columns list-gap-8">
                {{ range .JSON.Array "publicGroupList" }}
                {{ range .Array "monitorList" }}
                {{ $id := .String "id" }}
                {{ $hbArray := $hb.JSON.Array (print "heartbeatList." $id) }}
                <div class="flex items-center gap-12">
                  <a class="size-title-dynamic color-highlight text-truncate block grow" href="${UPTIME_KUMA_URL}/dashboard/{{ $id }}"
                    target="_blank" rel="noreferrer">
                    {{ .String "name" }} </a>

                  {{ if gt (len $hbArray) 0 }}
                    {{ $latest := index $hbArray (sub (len $hbArray) 1) }}
                    {{ if eq ($latest.Int "status") 1 }}
                    <div>{{ $latest.Int "ping" }}ms</div>
                    <div class="monitor-site-status-icon-compact" title="OK">
                      <svg fill="var(--color-positive)" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                        <path fill-rule="evenodd"
                          d="M10 18a8 8 0 1 0 0-16 8 8 0 0 0 0 16Zm3.857-9.809a.75.75 0 0 0-1.214-.882l-3.483 4.79-1.88-1.88a.75.75 0 1 0-1.06 1.061l2.5 2.5a.75.75 0 0 0 1.137-.089l4-5.5Z"
                          clip-rule="evenodd"></path>
                      </svg>
                    </div>
                    {{ else }}
                    <div><span class="color-negative">DOWN</span></div>
                    <div class="monitor-site-status-icon-compact" title="{{ if $latest.Exists "msg" }}{{ $latest.String "msg" }}{{ else
                      }}Error{{ end }}">
                      <svg fill="var(--color-negative)" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                        <path fill-rule="evenodd"
                          d="M8.485 2.495c.673-1.167 2.357-1.167 3.03 0l6.28 10.875c.673 1.167-.17 2.625-1.516 2.625H3.72c-1.347 0-2.189-1.458-1.515-2.625L8.485 2.495ZM10 5a.75.75 0 0 1 .75.75v3.5a.75.75 0 0 1-1.5 0v-3.5A.75.75 0 0 1 10 5Zm0 9a1 1 0 1 0 0-2 1 1 0 0 0 0 2Z"
                          clip-rule="evenodd"></path>
                      </svg>
                    </div>
                    {{ end }}
                  {{ else }}
                    <div><span class="color-negative">No data</span></div>
                    <div class="monitor-site-status-icon-compact" title="No data available">
                      <svg fill="var(--color-negative)" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                        <path d="M10 18a8 8 0 1 1 0-16 8 8 0 0 1 0 16zm0-2a6 6 0 1 0 0-12 6 6 0 0 0 0 12zm-.75-8a.75.75 0 0 1 1.5 0v3a.75.75 0 0 1-1.5 0V8zm.75 5a1 1 0 1 1 0 2 1 1 0 0 1 0-2z"/>
                      </svg>
                    </div>
                  {{ end }}
                </div>
                {{ end }}
                {{ end }}
              </ul>
              {{ end }}


          # steam specials
          - type: custom-api
            title: Steam Specials
            cache: 12h
            url: https://store.steampowered.com/api/featuredcategories?cc=us
            template: |
              <ul class="list list-gap-10 collapsible-container" data-collapse-after="5">
              {{ range .JSON.Array "specials.items" }}
                {{ $header := .String "header_image" }}
                {{ $urlPrefix := "https://store.steampowered.com/sub/" }}
                {{ if findMatch "/steam/apps/" $header }}
                  {{ $urlPrefix = "https://store.steampowered.com/app/" }}
                {{ end }}
                <li>
                  <a class="size-h4 color-highlight block text-truncate" href="{{ $urlPrefix }}{{ .Int "id" }}/">{{ .String "name" }}</a>
                  <ul class="list-horizontal-text">
                    <li>{{ .Int "final_price" | toFloat | mul 0.01 | printf "$%.2f" }}</li>
                    {{ $discount := .Int "discount_percent" }}
                    <li{{ if ge $discount 40 }} class="color-positive"{{ end }}>{{ $discount }}% off</li>
                  </ul>
                </li>
              {{ end }}
              </ul>


          # epic free game
          - type: custom-api
            title: Epic Games
            cache: 1h
            url: https://store-site-backend-static.ak.epicgames.com/freeGamesPromotions?locale=en&country=US&allowCountries=US
            template: |
              <div>
                {{ if eq .Response.StatusCode 200 }}
                  <div class="horizontal-cards-2">
                    {{ range .JSON.Array "data.Catalog.searchStore.elements" }}
                      {{ $price := .String "price.totalPrice.discountPrice" }}
                      {{ $hasPromo := gt (len (.Array "promotions.promotionalOffers")) 0 }}
                      {{ if and $hasPromo (eq $price "0") }}
                        {{ $gamePage := .String "productSlug" }}
                        {{ if gt (len (.Array "offerMappings")) 0 }}
                          {{ $gamePage = .String "offerMappings.0.pageSlug" }}
                        {{end }}
                        <a href="https://store.epicgames.com/en-US/p/{{ $gamePage }}" target="_blank" class="card">
                          {{ $title := .String "title" }}
                          {{ range .Array "keyImages" }}
                            {{ if eq (.String "type") "OfferImageWide" }}
                              <img src="{{ .String "url" }}" alt="{{ $title }}" style="width: 100%; max-width: 300px; height: 150px; object-fit: cover; border-radius: var(--border-radius);">
                            {{ end }}
                          {{ end }}
                          <div class="card-content">
                            <span class="size-base color-primary">{{ $title }}</span><br>
                            <span class="size-h5 color-subdue">
                              {{ if $hasPromo }}
                                {{ $promotions := .Array "promotions.promotionalOffers" }}
                                {{ if gt (len $promotions) 0 }}
                                  {{ $firstPromo := index $promotions 0 }}
                                  {{ $offers := $firstPromo.Array "promotionalOffers" }}
                                  {{ if gt (len $offers) 0 }}
                                    {{ $firstOffer := index $offers 0 }}
                                    Free until {{ slice ($firstOffer.String "endDate") 0 10 }}
                                  {{ else }}
                                    Free this week!
                                  {{ end }}
                                {{ else }}
                                  Free this week!
                                {{ end }}
                              {{ end }}
                            </span>
                          </div>
                        </a>
                      {{ end }}
                    {{ end }}
                  </div>
                {{ else }}
                  <p class="color-negative">Error fetching Epic Games data.</p>
                {{ end }}
              </div>

      - size: full
        widgets:
          - type: group
            widgets:
              - type: rss
                title: Corp Eng
                limit: 20
                collapse-after: 10
                cache: 12h
                feeds:
                  # Corp blogs
                  # RSS feeds: https://github.com/kilimchoi/engineering-blogs/blob/master/engineering_blogs.opml
                  # See this for insp: https://news.ycombinator.com/item?id=46363921
                  - url: https://tailscale.com/blog/index.xml
                    title: Tailscale Eng
                  - url: https://www.twilio.com/en-us/blog/feed
                    title: Twilio Eng
                    limit: 3
                  - url: https://engineering.fb.com/feed/
                    title: Meta Eng
                  - url: https://research.google/blog/rss/
                    title: Google Research 
                  - url: https://blog.cloudflare.com/rss/
                    title: Cloudflare Eng
                  - url: https://engineering.atspotify.com/feed/
                    title: Spotify Eng
                  - url: https://blog.palantir.com/feed
                    title: Palantir Eng
                  - url: https://medium.com/feed/airbnb-engineering
                    title: Airbnb Eng
                  - url: https://slack.engineering/feed/
                    title: Slack Eng
                  - url: https://stripe.com/blog/feed.rss
                    title: Stripe Eng
                  - url: https://dropbox.tech/feed
                    title: Dropbox Eng
                  - url: https://oxide.computer/blog/feed
                    title: Oxide Comp Eng
                  - url: https://www.linkedin.com/blog.rss
                    title: LinkedIn Eng
                  - url: https://blog.janestreet.com/feed.xml
                    title: Jane Street Eng
                  - url: https://tigerbeetle.com/blog/atom.xml
                    title: TigerBeetle

              # Personal Eng Blogs
              - type: rss
                title: Eng Blogs
                limit: 20
                collapse-after: 10
                cache: 12h
                feeds:
                  - url: https://www.seangoedecke.com/rss.xml
                    title: Sean Goedecke
                  - url: https://matklad.github.io/feed.xml
                    title: matklad
                  - url: https://ludic.mataroa.blog/rss/
                    title: Ludicity
                  - url: https://lucumr.pocoo.org/feed.xml
                    title: Armin Ronacher
                  - url: https://world.hey.com/dhh/feed.atom
                    title: DHH
                  - url: https://lethain.com/feeds.xml
                    title: Will Larson
                  - url: https://simonwillison.net/atom/entries/
                    title: Simon Willison

              - type: hacker-news
                limit: 20
                collapse-after: 10
              - type: lobsters
                limit: 20
                collapse-after: 10

          # Videos
          - type: group
            widgets: 
              - type: videos
                style: grid-cards
                collapse-after-rows: 3
                channels:
                  - UCUyeluBRhGPCW4rPe_UvBZQ # Primeagen
                  - UC6biysICWOJ-C3P4Tyeggzg # low level learning
                  - UC-mTIBh__DzAqW495JHXy5A # Coding Gopher
                  - UCavhLwXoB9tkqd-KsXpovOQ # jonathan blow highlights
                  - UCavhLwXoB9tkqd-KsXpovOQ # Wookash Podcast
                  - UCdM0LgFN54j-pRL-P0Ea_lg # LearnThatStack
                  - UCi8C7TNs2ohrc6hnRQ5Sn2w # programmers are human

      - size: small
        widgets:
          - type: weather
            location: Washington, District of Columbia, US
            units: imperial # alternatively "imperial"
            hour-format: 12h # alternatively "24h"
            # Optionally hide the location from being displayed in the widget
            # hide-location: true'

          - type: markets
            markets:
              - symbol: SPY
                name: S&P 500
              - symbol: QQQ
                name: NASDAQ
              - symbol: IWM
                name: Russell 2000
              - symbol: BTC-USD
                name: Bitcoin
              - symbol: GLD
                name: SPDR Gold

          # FED mortgage rates
          - type: custom-api
            title: 30-Year Fixed Rate Mortgage Avg
            cache: 12h
            url: https://api.stlouisfed.org/fred/series/observations?series_id=MORTGAGE30US&api_key=${FRED_API_KEY}&file_type=json&sort_order=desc&limit=2
            template: |
              {{ $latest := .JSON.Float "observations.0.value" }}
              {{ $previous := .JSON.Float "observations.1.value" }}
              {{ $lastObserve := .JSON.String "observations.0.date" }}
              {{ $change := sub $latest $previous }}
              <div class="flex justify-between items-center gap-15">
                <div class="min-width-0">
                  <a class="size-h3 block color-highlight" href="https://fred.stlouisfed.org/series/MORTGAGE30US" target="_blank" rel="noreferrer">
                    {{ .JSON.String "observations.0.value" }}%
                  </a>
                  <div class="text-truncate">St. Louis Federal Reserve</div>
                </div>
                <div class="shrink-0">
                  <div class="size-h3 text-right {{ if lt $change 0.0 }}color-positive{{ else if gt $change 0.0 }}color-negative{{ end }}">
                    {{ printf "%+.2f" $change }}%
                  </div>
                  <div class="text-right" title="Last change: {{ $lastObserve }}" {{ $lastObserve | parseRelativeTime "DateOnly" }}>
                  </div>
                </div>
              </div>

          - type: clock
            hour-format: 12h
            timezones:
              - timezone: America/New_York
                label: New York
              - timezone: America/Los_Angeles
                label: Seattle
              - timezone: Asia/Ho_Chi_Minh
                label: Vietnam
              - timezone: Europe/London
                label: London
              - timezone: Australia/Sydney
                label: Sydney

          - type: custom-api
            title: Random Fact
            cache: 3h
            url: https://uselessfacts.jsph.pl/api/v2/facts/random
            template: |
              <p class="size-h4 color-paragraph">{{ .JSON.String "text" }}</p>

          - type: custom-api
            title: Latest Community Widgets
            cache: 1d
            url: https://raw.githubusercontent.com/glanceapp/community-widgets/refs/heads/main/registry/widgets.json
            template: |
              <ul class="list list-gap-14 collapsible-container" data-collapse-after="3">
                {{ $widgets := .JSON.Array "" | sortByTime "time_added" "rfc3339" "desc" }}
                {{ if gt (len $widgets) 10 }}
                  {{ $widgets = slice $widgets 0 10 }}
                {{ end }}
                {{ range $widgets }}
                <li>
                  <a class="size-h4 block text-truncate color-primary-if-not-visited" href="https://github.com/glanceapp/community-widgets/blob/main/widgets/{{ .String "directory" }}/README.md" target="_blank" rel="noreferrer">{{ .String "title" }}</a>
                  <p class="text-truncate-2-lines margin-top-3">{{ .String "description" }}</p>
                  <ul class="list-horizontal-text margin-top-3">
                    <li {{ .String "time_added" | parseRelativeTime "rfc3339" }}></li>
                    <li>{{ .String "author" }}</li>
                  </ul>
                </li>
                {{ end }}
              </ul>

```

## Resources

*   [Official GitHub Repository](https://github.com/glanceapp/glance)
*   [Configuration Documentation](https://github.com/glanceapp/glance/blob/main/docs/configuration.md)
