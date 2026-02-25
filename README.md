# lobbyR \@ NICAR 2026

``` r
include=FALSE

knitr::opts_chunk$set(
  collapse = TRUE,
  comment = "#>",
  fig.path = "man/figures/README-",
  out.width = "100%"
)
```

# How to use federal lobbying disclosures?

Disclosures identify which lobbyists and organizations are trying to shape specific bills, agencies, and policies, revealing how private interests interact with public officials.

Knowing who is lobbying and on what issues helps the public and media understand the context behind legislative and regulatory decisions.

Here's some examples:

Bloomberg(Kate Ackley): [AI Lobbying Soars in Washington, Among Big Firms and Upstarts](https://news.bloombergtax.com/bloomberg-government-news/ai-lobbying-soars-in-washington-among-big-firms-and-upstarts)

Bloomberg(me): [Tax Prep, GOP-Aligned Groups Lobby to End IRS’ Direct File](https://news.bloombergtax.com/daily-tax-report/tax-prep-gop-aligned-groups-lobby-to-end-irs-direct-file)

Politico: [Inside the Warner Bros. lobbying bonanza](https://www.politico.com/news/2025/12/08/inside-the-warner-bros-lobbying-bonanza-00682276)

# Ways to sift through and structure the data

We can use the LDA site to search for [specific terms](https://lda.senate.gov/filings/public/filing/search/?registrant=&registrant_country=&registrant_ppb_country=&client=&client_state=&client_country=&client_ppb_country=&house_id=&lobbyist=&lobbyist_covered_position=&lobbyist_conviction_disclosure=&lobbyist_conviction_date_range_from=&lobbyist_conviction_date_range_to=&report_period=&report_year=&report_dt_posted_from=&report_dt_posted_to=&report_amount_reported_min=&report_amount_reported_max=&report_filing_uuid=&report_house_doc_id=&report_issue_area_description=%22Direct+file%22&affiliated_organization=&affiliated_organization_country=&foreign_entity=&foreign_entity_country=&foreign_entity_ppb_country=&foreign_entity_ownership_percentage_min=&foreign_entity_ownership_percentage_max=&search=search#js_searchFormTitle), [specific lobbyists](https://lda.senate.gov/filings/public/filing/search/?registrant=&registrant_country=&registrant_ppb_country=&client=&client_state=&client_country=&client_ppb_country=&house_id=&lobbyist=Bruce+Mehlman&lobbyist_covered_position=&lobbyist_conviction_disclosure=&lobbyist_conviction_date_range_from=&lobbyist_conviction_date_range_to=&report_period=&report_year=&report_dt_posted_from=&report_dt_posted_to=&report_amount_reported_min=&report_amount_reported_max=&report_filing_uuid=&report_house_doc_id=&report_issue_area_description=&affiliated_organization=&affiliated_organization_country=&foreign_entity=&foreign_entity_country=&foreign_entity_ppb_country=&foreign_entity_ownership_percentage_min=&foreign_entity_ownership_percentage_max=&search=search#js_searchFormTitle), [specific companies/firms/organizations](https://lda.senate.gov/filings/public/filing/search/?registrant=CHAMBER+OF+COMMERCE+OF+THE+U.S.A.&registrant_country=&registrant_ppb_country=&client=&client_state=&client_country=&client_ppb_country=&house_id=&lobbyist=&lobbyist_covered_position=&lobbyist_conviction_disclosure=&lobbyist_conviction_date_range_from=&lobbyist_conviction_date_range_to=&report_period=&report_year=&report_dt_posted_from=&report_dt_posted_to=&report_amount_reported_min=&report_amount_reported_max=&report_filing_uuid=&report_house_doc_id=&report_issue_area_description=&affiliated_organization=&affiliated_organization_country=&foreign_entity=&foreign_entity_country=&foreign_entity_ppb_country=&foreign_entity_ownership_percentage_min=&foreign_entity_ownership_percentage_max=&search=search#js_searchFormTitle), [specific topic areas](https://lda.senate.gov/filings/public/filing/search/?registrant=&registrant_country=&registrant_ppb_country=&client=&client_state=&client_country=&client_ppb_country=&house_id=&lobbyist=&lobbyist_covered_position=&lobbyist_conviction_disclosure=&lobbyist_conviction_date_range_from=&lobbyist_conviction_date_range_to=&report_period=&report_year=&report_dt_posted_from=&report_dt_posted_to=&report_amount_reported_min=&report_amount_reported_max=&report_filing_uuid=&report_house_doc_id=&report_issue_area=TAX&report_issue_area_description=&affiliated_organization=&affiliated_organization_country=&foreign_entity=&foreign_entity_country=&foreign_entity_ppb_country=&foreign_entity_ownership_percentage_min=&foreign_entity_ownership_percentage_max=&search=search) Note: I'm using the Senate's LDA site, but the House has its own version. They should have the same information.

We can use google docs functions like [importhtml to try and structure the data](https://docs.google.com/spreadsheets/d/1hn-jwRVPLR2_125uvciQQkr1j4exacUHSj7nQMQ3ejs/edit?usp=sharing).

But there are limitations-you get throttled at 6,000 results, and the information that can be brought in is limited.

# lobbyR

The goal of lobbyR is to provide a suite of tools for querying, cleaning, and analyzing U.S. Senate Lobbying Disclosure Act (LDA) data via the official REST API.

It is designed for journalists, researchers, and transparency advocates who want to explore federal lobbying disclosures in a reproducible and programmatic way.

The package includes helpers for searching by issue, client, registrant, and date, as well as for flagging duplicates, identifying client-registrant conflicts, and securely storing your API key.

### Installation

You can install the development version of lobbyR from [GitHub](https://github.com/Lobbying-DisclosuRe/lobbyr):

```{r}
devtools::install_github("Lobbying-DisclosuRe/lobbyr")
```

Or you can download it from CRAN!

```{r}
install.packages("lobbyR")
library(lobbyR)
```

Make sure the following packages are installed on your machine:

```{r}
pkgs <- c("httr2", "dplyr", "tidyr", "stringr", "keyring", "readr", "purrr", "devtools")
install.packages(setdiff(pkgs, rownames(installed.packages())))
lapply(pkgs, library, character.only = TRUE)
```

## [Click this to watch a video that walks you through the example](https://www.loom.com/share/7b96e863438b4d8fb220739b868c48aa?sid=a93a57dd-c82a-4472-97fd-5c50ddc163ee)

## Example

This is a basic example which shows you how to solve a common problem:

Here is [the visual representation](https://lda.senate.gov/filings/public/filing/search/?registrant=&registrant_country=&registrant_ppb_country=&client=7+eleven&client_state=&client_country=&client_ppb_country=&house_id=&lobbyist=&lobbyist_covered_position=&lobbyist_conviction_disclosure=&lobbyist_conviction_date_range_from=&lobbyist_conviction_date_range_to=&report_period=&report_year=&report_dt_posted_from=&report_dt_posted_to=&report_amount_reported_min=&report_amount_reported_max=&report_filing_uuid=&report_house_doc_id=&report_issue_area_description=&affiliated_organization=&affiliated_organization_country=&foreign_entity=&foreign_entity_country=&foreign_entity_ppb_country=&foreign_entity_ownership_percentage_min=&foreign_entity_ownership_percentage_max=&search=search#js_searchFormTitle) of what you're looking at if you were to search the Senate LDA website -

```{r}
#| label: fullexample

#load lobbyR
library(lobbyR)

# Set your API key (you'll be prompted to enter it securely)

if (FALSE) { # \dontrun{
  # just doing this so it doesn't run
  set_senate_api_key()
} # }

# Query filings for tax, company, or bill issues in the first quarter for a specific client/registrant
seven_eleven_filings <- get_filings(
  issues = c("fees", "foods", "immigration"),
  issue_joiner = "or",
  client_name = "7 Eleven, Inc.",
  ending_date = "2025-1-25", # format yyyy-mm-dd
  starting_date = "2020-04-01", # format yyyy-mm-dd
  tidy_result = TRUE,
  ignore_disclaimer = FALSE
)

# Flag and clean duplicate filings
dupes_flag_test <- flag_dupes(
  seven_eleven_filings,
  find_duplicates = TRUE,
  attempt_cleaning = TRUE
)

# Flag and remove potential double-counting between registrant and client
flagged_conflict <- flag_client_registrant_conflict(
  seven_eleven_filings,
  flag_conflict = TRUE,
  clean_doublecounts = TRUE
)
```

## A more complex example to get disclosures ready for textual analysis or to more simply eyeball it for patterns

Remember it's on lobbyists to divulge what and who they were lobbying – to there's no exact science. No silver bullet to get every disclosure. The key is to try and find the most common terms. This example already has done some of the

```{r}
#take a stab at some terms based on your reporting and then do a quick eyeball of how, what we're looking for, was being defined.

sec899_2025 <- get_filings(
  issues = c("899", "revenge tax", "unfair foreign tax", "Unfair Tax Prevention Act"),
  issue_joiner = "or",
  year = "2025",
  filing_period = NULL,
  client_name = NULL,
  tidy_result = F,
  ignore_disclaimer = T
) 

#This function filters out lobbying disclosures that don't mention tax and then simplify it in an easier-to-read format. Then it exports it to a .csv so we can look in excel/google docs

what_theyre_saying <- sec899_2025 |> 
  filter(`Taxation/Internal Revenue Code` != "NULL") |> 
  select("registrant.name", "client.name", "filing_type_display", "income", "expenses", "filing_year", "dt_posted", "filing_document_url", "registrant.description", "client.general_description", "filing_type", "filing_period",
  "Taxation/Internal Revenue Code") |> 
  mutate(`Taxation/Internal Revenue Code` = map_chr(`Taxation/Internal Revenue Code`, ~ str_c(.x, collapse = ", "))) |> 
  write_csv("whattheyresaying.csv")

#Now that we've honed our terms, Craft the API call to pull down the bills/terms we want to find for the Estes and Smith bills  
sec_899_estes_bill <- get_filings(
  issues = c("Unfair Tax Prevention Act", "(H.R. 2423)", "H.R.591", "Defending American Jobs and Investment Act"),
  issue_joiner = "or",
  filing_period = NULL,
  client_name = NULL,
  ending_date = "2025-07-23",
  starting_date = "2023-01-01",
  tidy_result = F,
  ignore_disclaimer = T
)

#go back through, filter the non-tax disclosures and simplify

no_tax_nas <- sec_899_estes_bill |> 
  filter(`Taxation/Internal Revenue Code` != "NULL") |> 
  select("registrant.name", "client.name", "filing_type_display", "income", "expenses", "filing_year", "dt_posted", "filing_document_url", "registrant.description", "client.general_description", "filing_type", "filing_period",
  "Taxation/Internal Revenue Code") |> 
  mutate(`Taxation/Internal Revenue Code` = map_chr(`Taxation/Internal Revenue Code`, ~ str_c(.x, collapse = ", "))) |> 
 write_csv("no_tax_nas.csv")

#i've written it out to a csv but here's where you can start getting creative with LLM, textual analysis, etc. We just need more than an hour for that :)  


```

## Main Functions

### `get_filings()`

-   Query the U.S. Senate LDA API for lobbying filings.
-   Search by issue(s), client, registrant, filing period, and date range.
-   Returns a cleaned and optionally tidied data frame.
-   Includes a detailed disclaimer and fact-checking guidance.

#### Example

```{r}
#| label: get_filings_example

chamber_df <- get_filings(
  issues = c("tax", "trade", "bill"),
  issue_joiner = "or",
  filing_period = "first_quarter",
  client_name = "Chamber of Commerce of the U.S.A.",
  registrant_name = "Chamber of Commerce of the U.S.A.",
  ending_date = "2025-01-25",
  starting_date = "2015-04-01",
  tidy_result = TRUE,
  ignore_disclaimer = TRUE
)
```

### `flag_dupes()`

-   Flags filings that may be duplicates, amendments, or otherwise dubious.
-   Adds diagnostic columns to help identify problematic filings.
-   Optionally removes all but the latest filing for each registrant-client-quarter group.

#### Example

```{r}
#| label: dupes_flag_test_example

dupes_flag_test <- flag_dupes(
  chamber_df,
  find_duplicates = TRUE,
  attempt_cleaning = TRUE
)
```

### `flag_client_registrant_conflict()`

-   Flags and (optionally) removes filings that could result in double-counting, such as when an entity files as both registrant and client.
-   Adds a flag column indicating which filings are likely to be duplicates.

#### Example

```{r}
#| label: flagged_conflict_example

flagged_and_clean_conflict <- flag_client_registrant_conflict(
  seven_eleven_filings,
  flag_conflict = TRUE,
  clean_doublecounts = TRUE
)
```

### `set_senate_api_key()`

-   Securely store your U.S. Senate LDA API key using the `keyring` package.
-   Prompts you to enter your key, which is then used by `get_filings()`.

#### Example

```{r}
#| label: set_senate_api_key_example
#| eval: false

set_senate_api_key()
```

### SHINY APP

I've also created a shiny app that you can tinker with that largely replicates the Senate's LDA page but does it in a way that's a little easier to parse. And I've built in many functions from the package into the app itself.

[You can find that here](https://github.com/Lobbying-DisclosuRe/lobbyR_NICAR/blob/main/lobby_r_app.R)

### MORE HELPFUL THINGS

A function using get fillings to iterate through a series of dates in order to create a complete picture of all lobbying disclosures for a given year, month or other specified time period.

```{r}
library(lobbyR)
library(tidyverse)


# - Saves each successful day's data to a temp file
# - Skips days with no data (doesn't create empty files)
# - Combines all files at the end
# - Cleans up temp files after combining
# - Returns NULL if no data was retrieved for any day

get_all_filings <- function(start_date = "2025-01-01", end_date =  format(Sys.Date(), "%Y-%m-%d")) {
#  format yyyy-mm-dd
  dates <- seq(as.Date(start_date), as.Date(end_date), by = "day")
  
  # Create temp directory
  temp_dir <- tempdir()
  
  # Get filings for each day and save to temp file
  walk(dates, function(date) {
    result <- tryCatch({
      get_filings(
        # year = as.character(year(date)),  # Extract year from date
        starting_date = as.character(date - 1),  # One day earlier
        ending_date = as.character(date),
        tidy_result = FALSE,
        ignore_disclaimer = TRUE
      )
    }, error = function(e) {
      message(paste("No data for:", date - 1, "to", date))
      return(NULL)
    })
    
    # Save only if we got data
    if (!is.null(result)) {
      filename <- file.path(temp_dir, paste0("filings_", date, ".rds"))
      saveRDS(result, filename)
      message(paste("Saved data for:", date - 1, "to", date))
    }
  })
  
  # Read all temp files and combine
  temp_files <- list.files(temp_dir, pattern = "filings_.*\\.rds$", full.names = TRUE)
  
  if (length(temp_files) > 0) {
    all_data <- map_dfr(temp_files, readRDS)
    
    # Clean up temp files
    file.remove(temp_files)
    
    return(all_data)
  } else {
    message("No data retrieved for any dates")
    return(NULL)
  }
}


# Usage #  format yyyy-mm-dd

q1_disclosures <- get_all_filings("2025-01-01", "2025-04-30") 
q2_disclosures <- get_all_filings("2025-05-01", "2025-08-31")
q3_disclosures <- get_all_filings("2025-09-01", "2025-12-31")
q4_disclosures <- get_all_filings("2026-01-01", "2026-02-06")
new_filings <- get_all_filings("2026-02-06", "2026-02-20")
#combine the files into a single dataframe 

full_filings <- rbind( q1_disclosures, q2_disclosures, q3_disclosures, q4_disclosures)

updated_full_filings <- rbind (full_filings, new_filings) #merge new query with existing filings to make sure there's no errors

full_filings <- updated_full_filings # write it back into the old file name so the rest of the code works, still 

write_rds(full_filings, "full_filings.rds") #save this 

write_csv(full_filings, "fullfilings.csv")

```

Function selects just the filings that lobbyists have entered text into the issue areas tax and transportation and then filter out all rows that just have null

```{r}

process_lobbying_data <- function(lobbying_df) {
  lobbying_df |> 
    select(c("registrant.name", "client.name", "filing_type_display", "income", "expenses", "filing_year", "dt_posted", "filing_document_url", "registrant.description", "client.general_description", "filing_type", "filing_period","Taxation/Internal Revenue Code", "Transportation")) |> 
    filter(filing_year == 2025) |> 
filter(`Taxation/Internal Revenue Code` != "NULL" |
           Transportation != "NULL") |> 
    mutate(across(everything(), ~as.character(replace(., is.na(.), "NULL")))) |> #turn null into characters 
  mutate(`Taxation/Internal Revenue Code` = map_chr(`Taxation/Internal Revenue Code`, ~paste(., collapse = ", "))) |>  #turn tax list into a character column 
 mutate(Transportation = map_chr(Transportation, ~paste(., collapse = ", "))) #turn list of transportation items into a character column
}

result <- process_lobbying_data(full_filings)


write_csv(result, "tax_transpo2025.csv")

```

This function does what I did above, but instead of specifying the column name in the function, you tell the function which the start and end columns. This allows us to run any and all issue areas.

```{r}


new_process_lobbying_data <- function(lobbying_df, start_col, end_col) {
  # Select columns
  selected_df <- lobbying_df |> 
    select(c("registrant.name", "client.name", "filing_type_display", "income", "expenses", 
             "filing_year", "dt_posted", "filing_document_url", "registrant.description", 
             "client.general_description", "filing_type", "filing_period",
             all_of(start_col:end_col)))
  
  # Get the names of the columns we want to process
  cols_to_process <- intersect(names(selected_df), names(lobbying_df)[start_col:end_col])
  
  # Print information for debugging
  cat("Total columns after selection:", ncol(selected_df), "\n")
  cat("Columns to process:", paste(cols_to_process, collapse = ", "), "\n")
  
  selected_df |> 
    filter(if_any(all_of(cols_to_process), ~!is.null(.) & . != "NULL")) |>
    mutate(across(everything(), ~as.character(replace(., is.na(.), "NULL")))) |>
    mutate(across(all_of(cols_to_process), 
                  ~map_chr(., function(x) paste(x, collapse = ", ")),
                  .names = "processed_{.col}"))
}

# Usage:
processed_result <- new_process_lobbying_data(full_filings, 65, 143)
#takes df, start_column, end_column

write_csv(processed_result, "lobbying_disclosures_for_all_topics.csv")

```

------------------------------------------------------------------------

## Data Cleaning and Best Practices

-   Always review flagged filings and potential double-counts by hand for critical analyses. There's lots of cases that don't get caught, unfortunately. For instance the 7 Eleven filing was spelled three different ways - "7-ELEVEN, INC", "7 ELEVEN, INC." and "7-ELEVEN, INC."
-   Use the `checkme` and `flag` columns to guide manual review. But there's always other cases. Feel free to flag and report them to me.
-   Only sum lobbying expenses for a registrant once per quarter/year to avoid double-counting.
-   Consult the filing document URL for source verification.

------------------------------------------------------------------------

## Disclaimer

> This data is known to contain errors and requires additional filtering and cleaning to ensure correct results. See documentation for more guidance and filtering examples. If you're looking to fact-check, use the filing document URL to review the source. Registrations and terminations are separate from quarterly lobby spending and must be filtered out to determine an entity's yearly spending on lobbying. If an entity appears as both registrant and client, do not sum the values; instead, use the registrant's expenses field to gauge total lobbying spend.

------------------------------------------------------------------------

## API Key

-   API keys can be requested at: <https://lda.senate.gov/api/register/>
-   Store your key securely with `set_senate_api_key()`.

------------------------------------------------------------------------

## Dependencies

-   `httr2`
-   `dplyr`
-   `tidyr`
-   `stringr`
-   `keyring`
-   `readr`
-   `purrr`

------------------------------------------------------------------------

## License

GNU LGPLv3

------------------------------------------------------------------------

## Contributing

Pull requests and issues are welcome. Please see the repository for guidelines.

------------------------------------------------------------------------

## Citation

Working on this still If you use this package in your research or reporting, please cite the data from the U.S. Senate Lobbying Disclosure Act Database and this package.

------------------------------------------------------------------------

## Contact

This package was developed by Chris Cioffi. For questions, issues, or suggestions, please visit:

------------------------------------------------------------------------

## Acknowledgments

Thanks to the U.S. Congress for providing open data and the guidance of American University's Aarushi Sahejpal. Thanks to AI tools and the R community for help and how-tos in creating some regexes and code patterns.
