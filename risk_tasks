import requests
import json
import pandas as pd
import math

number_of_tasks = int(input("Enter number of tasks: "))
# number_of_tasks = 2

tasks = []
for task in range(number_of_tasks):
    task_name = input("Enter Task Name (ie Attack Ann Arbor): ")
    percentage = int(input("Enter percentage of total forces for this task (go in order of most to least): "))
    tasks.append([task_name, percentage])
# tasks = [["attack ann arbor", 70], ["defend ohio state", 30]]


def get_players(team):
    try:
        response = requests.get(f'https://collegefootballrisk.com/api/players?team={team}')
        response.raise_for_status()
        return json.loads(response.content)
    except requests.exceptions.RequestException as e:
        print(e)
        exit()


def create_dict(players):
    d = {}
    for player in players:
        name = player["player"]
        star_power = player["lastTurn"]["stars"]
        d[name] = star_power
    return d


def sort_dict(d):
    sorted_dict = sorted(d.items(), key=lambda x: x[1], reverse=True)
    return dict(sorted_dict)


# def final_dict(player_dict, total_stars, tasks, percentages):
#     final_d = {};
#     for player in dict:

def create_csv(d, file_name):
    df = pd.DataFrame(d)
    df.to_csv(file_name, index=False, header=None)


team = "Ohio%20State"
players = get_players(team)
player_dict = create_dict(players)
total_stars = sum(player_dict.values())
sorted_dict = sort_dict(player_dict)

final_list = []
current_task = 0
task_star_percentage = float(tasks[current_task][1]) / 100
stars_per_task = math.floor(total_stars * task_star_percentage)
stars_remaining = stars_per_task
for player in range(len(sorted_dict)):
    player_name = list(sorted_dict.keys())[player]
    player_star_value = sorted_dict[player_name]
    if stars_remaining > 0 or current_task >= number_of_tasks:
        final_list.append([player_name, tasks[current_task][0]])
        stars_remaining = stars_remaining - player_star_value
    else:
        current_task = current_task + 1
        try:
            task_star_percentage = float(tasks[current_task][1]) / 100
        except Exception as exc:
            current_task = current_task - 1
        stars_per_task = math.floor(total_stars * task_star_percentage)
        stars_remaining = stars_per_task
        final_list.append([player_name, tasks[current_task][0]])
        stars_remaining = stars_remaining - player_star_value


create_csv(final_list, "orders.csv")

