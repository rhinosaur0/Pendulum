# As part of the pendulum project, I created a script that reads values of time and period from Tracker Video Analysis, and converts it to a list of tuples of the form (release angle, period)

def text_to_tuples(filename):
    tuples_list = []
    with open(filename, 'r') as file:
        for line in file:
            numbers = line.split()
            tuple_pair = (float(numbers[0]), float(numbers[1]))
            tuples_list.append(tuple_pair)
    return tuples_list

filename = 'dataset.txt'  # Replace with your actual file path
tuples = text_to_tuples(filename)

time_tracker = [tuples[0][0]]
theta_tracker = [tuples[0][1]]
up_or_down = 1
endpoints = []


# Recognizes the maximum displacement for each side of the pendulum, storing them as (time, angle)
for a, b in tuples:
    
  if up_or_down == 1:
      if b > theta_tracker[-1]:
          theta_tracker[-1] = b
      else:
          endpoints.append((a, b))
          up_or_down = 0
          time_tracker.append(a)

  else:
      if b < theta_tracker[-1]:
          theta_tracker[-1] = b
      else:
          endpoints.append((a, b))
          up_or_down = 1
          time_tracker.append(a)

result = []

# Since the maximum displacement alternates between positive and negative, endpoints[i+2][0] - endpoints[i][0] yields the period for that specific angle
for i in range(2, len(endpoints) - 2):
    result.append((endpoints[i][1], endpoints[i + 2][0] - endpoints[i][0])) 
