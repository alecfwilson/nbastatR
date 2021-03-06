# nbastatR
An interface for professional basketball data in R.  Data sources include, but are not limited to: NBA Stats API, Basketball Insiders, Basketball-Reference, HoopsHype, and RealGM.  Overtime additional data sources will be added.

This package is in it's extreme infancy but it should work if you have the necassary packages installed.  Formal vigentte's will come once the wrappers are complete but you please explore around.  Here is some sample code encompassing some of what nbastatR can do.  Once the nbastats API is fully wrapped full documentation will come.
## Installation
```{r}
devtools::install_github("abresler/nbastatR")
library("nbastatR") # note requires a bunch of other packages which are listed in the import
```

## Explore A Season with the NBA Stats API 
```{r}
all_nba_players <- 
	get_nba_players_ids(league = "NBA",
											active_only = F)

players_1998 <-
	get_nba_season_players(
		year.season_start = 1998,
		include_only_rostered_players = F,
		return_message = T
	)

players_1998_2015 <- get_nba_seasons_players(years = 1998:2014,
																						 only_on_roster = T,
																						 message = F)

profiles_2015_season <-
	get_season_player_profiles(year.season_start = 2014, include_headline_stats = T,
														 only_rostered_players = T,
														 message = T)


```
## Other Functionality
```{r}
get_day_nba_games("02/11/2016")
get_day_nba_game_scores(date = "10/06/15")
get_day_nba_game_team_leaders("02/08/16")
get_nba_franchise_data(return_franchises = "all")
get_nba_team_stat_table(year.season_start = 2015,
                        period = 4,
                        division_against = "Atlantic",
                        outcome = "W")
get_nba_player_injuries(filter_returning_today = T)
get_nba_synergy_stats(table_name = "Isolation",
                      include_defense = T,
                      include_offense = T,
                      type_table = 'player') # Player isolations
get_nba_synergy_stats(table_name = "Transition",
           include_defense = T,
           include_offense = T,
           type_table = "team", #team transition
           return_message = T)
  
```

## NBA Stats Beta NLP API
This function allows you to ask the NBA's new NLP API a question and it will return an answer in a data frame when possible.
```{r}
ask_nba_api_nlp_question(question = "BROOKLYN NETS WINS ALL TIME",
                         return_similar_questions = T)

ask_nba_api_nlp_question(question = "Most points ever scored in a game",
                         return_similar_questions = T)

ask_nba_api_nlp_question(question = "Rudy Gobert Free Throw Percentage All Time",
                         return_similar_questions = T)

ask_nba_api_nlp_question(question = "Derrick Rose points against LeBron James")
questions <- 
  c('Wayne Ellington catch and shoot field goals', 'Joe Johnson catch and shoot field goals')
questions %>% 
  ask_nba_api_nlp_questions()

```

## NBA Draft Functions
``` {r}
get_year_draft_combine(combine_year = 2009, return_message = T)
get_all_draft_combines(combine_years = 2000:2015) #draft combines
get_all_draft_data(draft_years = 1960:2015) # drafts
```

## Salary Cap Data Example

```{r}
all_salaries <- 
  get_all_team_salaries()

non_guaranteed_players <- 
  all_salaries %>% 
  dplyr::filter(id.season %in% c("2015-16", "2016-17"),
                is.non.guaranteed == T)

non_guaranteed_players %>% 
  View()

team_2017_non_guaranteed_players <- 
  non_guaranteed_players %>% 
  group_by(id.season, team) %>% 
  summarise(players = n(),
            total.non_guaranteed.salary = sum(value, na.rm = T) %>% formattable::currency(digits = 0)
            ) %>% 
  ungroup() %>% 
  arrange(desc(total.non_guaranteed.salary))

team_2017_non_guaranteed_players %>% 
  View()
```

## Coaching Data Examples

```{r}
get_bref_coach_transaction_df(coach = "David Blatt")
get_bref_coach_bio_df("Steve Kerr")
get_bref_coach_stat_df("Monty Williams")
get_bref_coach_award_df("Gregg Popovich")
```


## Mock Draft
```{r}
draft_2016 <- 
  get_nba_draftnet_year_mock_draft(draft_year = 2016)

mocks_09_15 <- 
  get_nba_draftnet_years_mock_draft(draft_years = 2009:2015)
```
