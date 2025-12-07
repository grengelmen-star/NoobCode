package main

import (
	"fmt"
	"math/rand"
	"time"
)

const (
	Height          = 30
	Width           = 30
	Generation      = 50
	GenerationAlive = 0.30
	TimeSleepMs     = 500
)

type Field [Height][Width]bool

func main() {
	ClearScreen()
	var field Field

	fmt.Println("Выбери стартовую конфигурацию")
	fmt.Println("1 - Классическая игра")
	fmt.Println("2 - Глайдер")

	var choiсe int
	fmt.Print("Ты выбрал (1 или 2):")
	fmt.Scan(&choiсe)

	if choiсe == 1 {
		field = InitStartField()
		fmt.Print("Создано случайное поле")
	} else if choiсe == 2 {
		field = CreateGlider()
		fmt.Print("Создан глайдер")
	} else {
		fmt.Println("Ошибка, такого варианта не было, играй в классику")
		field = InitStartField()
	}
	time.Sleep(2 * time.Second)
	for gen := 0; gen < Generation; gen++ {
		ClearScreen()
		fmt.Printf("Поколение %d: | Живых клеток %d: | Всего клеток %d:", gen, AliveCountVisible(field), Height*Width)
		PrintField(field)
		field = NextGeneration(field)
		time.Sleep(TimeSleepMs * time.Millisecond)
	}
}
func InitStartField() Field {
	var field Field
	for y := 0; y < Height; y++ {
		for x := 0; x < Width; x++ {
			field[y][x] = rand.Float64() < GenerationAlive
		}
	}
	return field
}
func PrintField(f Field) {
	for y := 0; y < Height; y++ {
		for x := 0; x < Width; x++ {
			if f[y][x] { //если клетка жива - рисуем квадрат
				fmt.Print("\033[32m■\033[0m")
			} else {
				fmt.Print(" ") //если мертва - пусто
			}
		}
		fmt.Println()
	}
}
func min(a, b int) int { //вспомогательная функция
	if a < b {
		return a
	}
	return b
}
func max(a, b int) int { //вспомогательная функция
	if a > b {
		return a
	}
	return b
}
func CountNeighbors(f Field, y, x int) int { // подсчет соседей
	count := 0
	for ny := max(y-1, 0); ny <= min(y+1, Height-1); ny++ {
		for nx := max(x-1, 0); nx <= min(x+1, Width-1); nx++ {
			if ny == y && nx == x { // если это сама клетка - продолжаем
				continue
			}
			if f[ny][nx] { // если сосед жив - увеличиваем счетчик
				count++
			}
		}
	}
	return count
}
func NextGeneration(current Field) Field { //основная функция для создания следующего поля и применения правил игры
	var next Field
	for y := 0; y < Height; y++ {
		for x := 0; x < Width; x++ {
			AliveNeighbors := CountNeighbors(current, y, x)
			if current[y][x] { // если клетка жива - применяем правила
				next[y][x] = AliveNeighbors == 2 || AliveNeighbors == 3 // два или три соседа - выживает
			} else { //иначе(клетка мертва)
				next[y][x] = AliveNeighbors == 3 //при наличии трех соседей - оживает
			}
		}
	}
	return next
}

// Далее идут фичи, они не обязательны для нашей игры, однако делают ее более полноценной и интересной
func AliveCountVisible(f Field) int {
	count := 0
	for y := 0; y < Height; y++ {
		for x := 0; x < Width; x++ { //классический цикл для проверки всех клеток
			if f[y][x] { //если клетка жива - увеличиваем счетчик, который в будущем выведем на экран
				count++
			}
		}
	}
	return count
}
func ClearScreen() {
	fmt.Print("\x1b[H\x1b[2J")
}
func CreateEmptyField() Field {
	return Field{}
}
func CreateGlider() Field {
	var field Field

	centerY := Height / 2
	centerX := Width / 2

	field[centerY][centerX+1] = true
	field[centerY+1][centerX+2] = true
	field[centerY+2][centerX] = true
	field[centerY+2][centerX+1] = true
	field[centerY+2][centerX+2] = true
	return field
}
