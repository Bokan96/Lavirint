## Lavirint

Download link sa Google Drive-a: [Download](https://drive.google.com/file/d/1TtzQ5N6_SucjwUfoNKokzYS1jVhIkufn/view?usp=sharing)

### Osnovna logika bez pratecih klasa je ispod

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown

extends TileMap

export var generate_on_ready = true
export var  LAVIRINT_VELICINA = Vector2(31,17) ## 31x17
export var  TRENUTNA_POZICIJA = Vector2(1, 1)
export var ZID_ID = 0
export var PUT_ID = 1
export var IVICA_ID = 2
export var HEROJ_ID = 3
const SMEROVI = [
	Vector2.UP * 2,
	Vector2.DOWN * 2,
	Vector2.RIGHT * 2, ## Sinonim za Vektor (2,0)
	Vector2.LEFT * 2,
]
var trenutno_polje = Vector2.ONE
var posecena_polja = [trenutno_polje]
var stack = []
var polja = []

func _ready():
	trenutno_polje += TRENUTNA_POZICIJA - Vector2.ONE
	if generate_on_ready:
		generisi_lavirint()

func _input(event):
	if event.is_action_pressed("ui_accept"):
		generisi_lavirint()

### Generisanje lavirinta ###

func generisi_lavirint():
	#Ocisti sve
	posecena_polja = [trenutno_polje]
	stack.clear()
	# Create walls
	for x in LAVIRINT_VELICINA.x:
		for y in LAVIRINT_VELICINA.y:
			set_cell(x+TRENUTNA_POZICIJA.x, y+TRENUTNA_POZICIJA.y, ZID_ID)
	# Create limits
	for x in LAVIRINT_VELICINA.x+2:
		set_cell(x+TRENUTNA_POZICIJA.x-1, TRENUTNA_POZICIJA.y-1, IVICA_ID)
		set_cell(x+TRENUTNA_POZICIJA.x-1, TRENUTNA_POZICIJA.y-2, IVICA_ID)
	for x in LAVIRINT_VELICINA.x+2:
		set_cell(x+TRENUTNA_POZICIJA.x-1, LAVIRINT_VELICINA.y + TRENUTNA_POZICIJA.y, IVICA_ID)
		set_cell(x+TRENUTNA_POZICIJA.x-1, LAVIRINT_VELICINA.y + TRENUTNA_POZICIJA.y+1, IVICA_ID)
	for y in LAVIRINT_VELICINA.y:
		set_cell(TRENUTNA_POZICIJA.x-1, TRENUTNA_POZICIJA.y + y , IVICA_ID)
		set_cell(TRENUTNA_POZICIJA.x-2, TRENUTNA_POZICIJA.y + y , IVICA_ID)
	for y in LAVIRINT_VELICINA.y:
		set_cell(LAVIRINT_VELICINA.x + TRENUTNA_POZICIJA.x, TRENUTNA_POZICIJA.y + y , IVICA_ID)
		set_cell(LAVIRINT_VELICINA.x + TRENUTNA_POZICIJA.x+1, TRENUTNA_POZICIJA.y + y , IVICA_ID)
	#create polja
	for x in (LAVIRINT_VELICINA.x+1)/2:
		for y in (LAVIRINT_VELICINA.y + 1)/2:
			set_cell(TRENUTNA_POZICIJA.x + 2 * x , TRENUTNA_POZICIJA.y + 2 * y, PUT_ID)
	#get cells
	polja = get_used_cells_by_id(PUT_ID)
	#generate maze
	while posecena_polja.size() < polja.size():
		var neighbours = susedna_polja_nisu_posecena(trenutno_polje)
		if neighbours.size() > 0:
			var random_neighbour = neighbours[randi()%neighbours.size()]
			stack.push_front(trenutno_polje)
			var wall = (random_neighbour - trenutno_polje)/2 + trenutno_polje
			set_cell(int(wall.x), int(wall.y), 1)
			trenutno_polje = random_neighbour
			posecena_polja.append(trenutno_polje)
		elif stack.size() > 0:
			trenutno_polje = stack[0]
			stack.pop_front()

func susedna_polja_nisu_posecena(cell):
	var neighbours = []
	for dir in SMEROVI:
		if not posecena_polja.has(cell + dir) and get_cell(int(cell.x + dir.x), int(cell.y + dir.y)) != IVICA_ID:
			neighbours.append(cell + dir)
	return neighbours


```
