
# Streamlit using Docker

Now we will Deploy our Streamlit app using Docker which is independent platform and used in many places..

Now we study about the documentation of Docker and stremlit




## Documentation


[Docker](https://www.docker.com/products/docker-desktop/)

[Streamlit](https://docs.streamlit.io/)

[Docker Desktop](https://www.docker.com/products/docker-desktop/)







## Installation

Now first of all we will install the streamlit and Docker in our system to do.

To install streamlit
```bash
 pip install streamlit
```
Now you also need to install Docker Desktop with the above Docker Desktop link

After that we will check if the Docker and Python is install in our system.

```bash
 Docker --version
```

To check for python:
```bash
 python --version
```




## Deployment

Now finally we have all the things for deployment which includes Python, Docker and Streamlit.

First of all we will tke the streamlit.py app from the website

```bash
  from collections import namedtuple
import altair as alt
import math
import pandas as pd
import streamlit as st

"""
# Welcome to Streamlit!

Edit `/streamlit_app.py` to customize this app to your heart's desire :heart:

If you have any questions, checkout our [documentation](https://docs.streamlit.io) and [community
forums](https://discuss.streamlit.io).

In the meantime, below is an example of what you can do with just a few lines of code:
"""

with st.echo(code_location='below'):
   total_points = st.slider("Number of points in spiral", 1, 5000, 2000)
   num_turns = st.slider("Number of turns in spiral", 1, 100, 9)

   Point = namedtuple('Point', 'x y')
   data = []

   points_per_turn = total_points / num_turns

   for curr_point_num in range(total_points):
      curr_turn, i = divmod(curr_point_num, points_per_turn)
      angle = (curr_turn + 1) * 2 * math.pi * i / points_per_turn
      radius = curr_point_num / total_points
      x = radius * math.cos(angle)
      y = radius * math.sin(angle)
      data.append(Point(x, y))

   st.altair_chart(alt.Chart(pd.DataFrame(data), height=500, width=500)
      .mark_circle(color='#0068c9', opacity=0.5)
      .encode(x='x:Q', y='y:Q'))
b. If 
```

Now we will also copy the Docker file for the same.

```bash
# app/Dockerfile

FROM python:3.9-slim

WORKDIR /app

RUN apt-get update && apt-get install -y \
    build-essential \
    curl \
    software-properties-common \
    git \
    && rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/streamlit/streamlit-example.git .

RUN pip3 install -r requirements.txt

EXPOSE 8501

HEALTHCHECK CMD curl --fail http://localhost:8501/_stcore/health

ENTRYPOINT ["streamlit", "run", "streamlit_app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```
 Then install your app’s Python dependencies from the cloned requirements.txt in the container:

 ``` bash
 RUN pip3 install -r requirements.txt
 ```

After setting up all the things we will now build the app in docker engine for that we will run the following commands.
The -t flag is used to tag the image. Here, we have tagged the image streamlit

``` bash
docker build -t streamlit_app .
```

Then to confirm we will check if the app is install or not.

```bash
docker images
```
You should see a streamlit image under the REPOSITORY column. For example:

```bash
REPOSITORY   TAG       IMAGE ID       CREATED              SIZE
streamlit    latest    70b0759a094d   About a minute ago   1.02GB
```

In the last we will run the app using the following commands.

```bash
docker run -p 8501:8501 streamlit
```

