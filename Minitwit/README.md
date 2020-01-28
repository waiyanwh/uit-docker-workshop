## Bonus App

MiniTwit

because writing todo lists is not fun

#### What is MiniTwit?

A SQLite and Flask powered twitter clone
#### Docker instructions

To create a docker image execute:

```bash
cd minitwit
docker build . -t minitwit
```

To run the docker image execute:

```bash
docker run -p 5000:5000 minitwit
```
and visit with your browser http://localhost:5000
