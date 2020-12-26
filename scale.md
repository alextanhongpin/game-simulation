```go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func init() {
	rand.Seed(time.Now().Unix())
}
func main() {
	// During each round, each player choose a number between 0-99.
	// At the end of the round, the average of the number * 0.8 will be computed.
	// The player closest to the number is the winner.
	// The losers will be deducted a point.
	// A player retires if the points drop below -10.
	// The game continues until there is a winner left.
	n := 5
	players := make([]int, n)
	scores := make([]int, n)
	var i int
	for n != 1 {
		i++
		var total int
		for j := 0; j < n; j++ {
			players[j] = rand.Intn(100)
			total += players[j]
		}
		fmt.Println("Round", i)
		fmt.Println("Players choose", players)
		average := total / n * 4 / 5
		fmt.Println("Average:", average)

		for _, v := range players {
			if delta := abs(average - v); delta < total {
				total = abs(average - v)
			}
		}
		var indices []int
		for k, v := range players {
			if total == abs(average-v) {
				fmt.Println("Player", k+1, "win")
				continue
			}

			scores[k]--
			if scores[k] == -10 {
				n--
				fmt.Println("Player", k+1, "retires")
				indices = append(indices, k)

			} else {
				fmt.Println("Player", k+1, "lose")
			}
		}
		for _, d := range indices {
			scores = append(scores[:d], scores[d+1:]...)
			players = append(players[:d], players[d+1:]...)
		}
		fmt.Println("Scores:", scores)
		fmt.Println()
	}
}

func abs(a int) int {
	if a < 0 {
		return -a
	}
	return a
}
```
