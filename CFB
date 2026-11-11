import pandas as pd


# Removes the conference from the end of a team name.
# "BYU (FBS Independent)" becomes "BYU", and "Miami (FL) (ACC)" becomes
# "Miami (FL)". rfind() works like find() but searches from the right,
# so only the last set of parentheses is removed.
def remove_conference(sTeam):
    iSpot = sTeam.rfind(" (")
    if iSpot == -1:
        return sTeam
    return sTeam[:iSpot]


# Returns the name of the win category a team belongs in
def get_category(iWins):
    if iWins >= 10:
        return "10 or more wins"
    elif iWins >= 8:
        return "8–9 wins"
    elif iWins >= 6:
        return "6–7 wins"
    else:
        return "Fewer than 6 wins"


# Builds a dictionary that holds a list of teams for each category.
# The keys must match the names returned by get_category()
# (the dash in "8–9" and "6–7" is the same long dash used there).
def organize_teams(dfSeason):
    dictCategories = {
        "10 or more wins": [],
        "8–9 wins": [],
        "6–7 wins": [],
        "Fewer than 6 wins": []
    }

    # Loop through every team in the season and file it by its wins.
    # iterrows() gives the row number too, but only the row is needed.
    for iIndex, row in dfSeason.iterrows():
        sCategory = get_category(row["Win"])
        dictCategories[sCategory].append(remove_conference(row["Team"]))

    return dictCategories


# Prints each category heading followed by its teams
def display_results(iSeason, dictCategories):
    print(f"College Football Results for {iSeason}")
    for sCategory, lstTeams in dictCategories.items():
        print(f"{sCategory}:")
        if len(lstTeams) == 0:
            print("(no teams)")
        for sTeam in lstTeams:
            print(sTeam)


# Returns the seasons in the data as text, like "2013, 2014, 2015".
# The seasons come from the file, so none are hard-coded.
def get_season_list(dfFootball):
    lstSeasonText = []
    for iYear in sorted(dfFootball["Season"].unique()):
        lstSeasonText.append(str(iYear))
    return ", ".join(lstSeasonText)


# Asks the user a question. Returns None if they press Ctrl-C or the
# input closes (Ctrl-D), so the program can end without a crash.
def ask(sPrompt):
    try:
        return input(sPrompt)
    except (EOFError, KeyboardInterrupt):
        return None


def main():
    # cfb.csv must be in the folder the program is run from
    try:
        dfFootball = pd.read_csv("cfb.csv")
    except FileNotFoundError:
        print("Could not find cfb.csv. Put it in the same folder as football.py and run the program from that folder.")
        return

    sSeasonList = get_season_list(dfFootball)

    # Keep asking until the user enters a season that is in the data
    bFound = False
    while not bFound:
        sSeason = ask("Enter a season to analyze: ")
        if sSeason is None:
            print("\nGoodbye!")
            return

        try:
            iSeason = int(sSeason)
        except ValueError:
            print(f"'{sSeason}' is not a year. Available seasons: {sSeasonList}\n")
            continue

        # Keep only the rows for the season the user chose
        dfSeason = dfFootball[dfFootball["Season"] == iSeason]

        if dfSeason.empty:
            print(f"Season {iSeason} was not found in the data.")
            print(f"Available seasons: {sSeasonList}\n")
        else:
            bFound = True

    # Most wins first; teams with the same wins in alphabetical order
    dfSeason = dfSeason.sort_values(["Win", "Team"], ascending=(False, True))

    dictCategories = organize_teams(dfSeason)
    display_results(iSeason, dictCategories)


main()
