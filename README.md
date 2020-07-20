#-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-#
#        ██ ▄█▀ ██▓    ▓█████▄ ▓█████ ██▒   █▓        #
#        ██▄█▒ ▓██▒    ▒██▀ ██▌▓█   ▀▓██░   █▒        #
#       ▓███▄░ ▒██░    ░██   █▌▒███   ▓██  █▒░        #
#       ▓██ █▄ ▒██░    ░▓█▄   ▌▒▓█  ▄  ▒██ █░░        #
#       ▒██▒ █▄░██████▒░▒████▓ ░▒████▒  ▒▀█░          #
#       ▒ ▒▒ ▓▒░ ▒░▓  ░ ▒▒▓  ▒ ░░ ▒░ ░  ░ ▐░          #
#       ░ ░▒ ▒░░ ░ ▒  ░ ░ ▒  ▒  ░ ░  ░  ░ ░░          #
#       ░ ░░ ░   ░ ░    ░ ░  ░    ░       ░░          #
#       ░  ░       ░  ░   ░       ░  ░     ░          #
#                       ░                 ░           #
#-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-#
options:
	versao: v10.4
#	Autor: Kick Holse e uOnyle | Nome: KhKits
#━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━#
# options e umas functions
options:
	groups-file: plugins/KhKits/groups.yml
	config-file: plugins/KhKits/config.yml
	lang-file: plugins/KhKits/languages.yml
	kits-file: plugins/KhKits/kits.yml
	players-file: plugins/KhKits/players/%player%.yml
	playerc-file: plugins/KhKits/players/%{_p}%.yml
	playerv-file: plugins/KhKits/players/%victim%.yml
	playera-file: plugins/KhKits/players/%attacker%.yml
#━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━#
# funções
function showScoreboard(p: player):
	delete stylish scoreboard "SCORE-%{_p}%"
	create new stylish scoreboard named "SCORE-%{_p}%"
	set {_line} to 1
	loop {score.lines} times:
		create a new id based score "SCORE-%{_p}%Slot%{_line}%" with text "&%{_line}%" slot {_line} for stylish scoreboard "SCORE-%{_p}%"
		add 1 to {_line}
	while {_p} is online:
		set stylish scoreboard of {_p} to "SCORE-%{_p}%"		
		set title of stylish scoreboard "SCORE-%{_p}%" to getText({_p}, "%{score.title}%")
		set {_line} to 1
		loop {score.lines} times:
			set the text of id "SCORE-%{_p}%Slot%{_line}%" to getText({_p}, "%{score.line::%{_line}%}%")
			add 1 to {_line}
		wait 1 seconds
function khkits_join(p: player, ta: text, te: text):
	if {_ta} is "update":
		if {_te} is "tablist":
			if {khkp::tab.enabled} is true:
				set tab header to colored {khkp::tab.header} and footer to colored {khkp::tab.footer} for {_p}
	if {_ta} is "hotbar":
		if {_te} is "cleareffects":
			clear {zen.%{_p}%} and {quick.%{_p}%} and {antistomper.%{_p}%} and {espinhos.%{_p}%} and {stomper.%{_p}%} and {fisherman.%{_p}%} and {darkness.%{_p}%} and {snail.%{_p}%} and {ninja-time.%{_p}%} and {ninja-victim.%{_p}%} and {ninja.%{_p}%} and {phanton.%{_p}%} and {fogareu.%{_p}%} and {TimeLord.%{_p}%} and {Monk.%{_p}%} and {Kangaro.%{_p}%} and {Thor.%{_p}%} and {Camel.%{_p}%} and {Fly.%{_p}%} and {Invencivel.%{_p}%} and {Viper.%{_p}%}
			set {_p}'s max health to 10
			heal {_p}
			extinguish {_p}
			set {_p}'s food to 10
			clear {_p}'s level
			clear {_p}'s inventory
			milk {_p}
		if {_te} is "soups":
			give {_p} 36 soup named colored {khkp::soup-name}
			set slot 13 of {_p} to 64 red mushroom named colored {khkp::rmush-name}
			set slot 14 of {_p} to 64 brown mushroom named colored {khkp::bmush-name}
			set slot 15 of {_p} to 64 bowl named colored {khkp::pot-name}
		if {_te} is "itens":
			loop "hotbar-warps" and "hotbar-kits" and "hotbar-shop":
				set {_slot} to getSlot({khkp::%loop-value%}) parsed as int
				set {_item} to getItem({khkp::%loop-value%})
				if {_item} contains "HEAD:owner=":
					set {_x} to last element of {_item} split at "HEAD:owner=" parsed as offline player
					if {_x} is "{player}":
						replace all "{player}" in {_x} with "%{_p}%"
						set {_x} to "%{_x}%" parsed as player
					set slot {_slot} of {_p} to {_x}'s skull named getName({khkp::%loop-value%})
				else if {_item} contains "SHINY ":
					replace all "SHINY " in {_item} with ""
					set {_item} to "%{_item}%" parsed as material
					set slot {_slot} of {_p} to shiny {_item} named getName({khkp::%loop-value%})
				else:
					set {_item} to "%{_item}%" parsed as material
					set slot {_slot} of {_p} to {_item} named getName({khkp::%loop-value%})
function khkits_menus(p: player, type: text):
	if {_type} is "warps":
		wait 3 tick
		set {fps-online} to number of all players
		set {knock-online} to number of all players
		set {lava-online} to number of all players
		loop all players:
			set {_kit-sel.%loop-player%} to yml value "%uuid of loop-player%.selected-kit" of file "plugins/KhKits/players/%loop-player%.yml"
			if {_kit-sel.%loop-player%} is not "%{khkp::warpfps}%":
				remove 1 from {fps-online}
			if {_kit-sel.%loop-player%} is not "%{khkp::warpknock}%":
				remove 1 from {knock-online}
			if {_kit-sel.%loop-player%} is not "%{khkp::warplava}%":
				remove 1 from {lava-online}
		open chest with 3 row named "Warps" to {_p}
		play sound "CLICK" to {_p} with volume 0.5 and pitch 15.0
		wait 1 tick
		format slot 10 of {_p} with plain glass block named "&aFPS" with lore "&7Esta warp é ideal para aqueles||&7jogadores que possuem uma máquina mais||&7humilde. Jogue aqui sem problemas com fps!|| ||&7%{fps-online}% jogando!||&eClique aqui para ir até a warp!" to close then run [make {_p} execute command "fps"]
		format slot 12 of {_p} with shiny stick named "&aKNOCKBACK" with lore "&7Nesta warp você poderá jogar||&7seus amigos para mem longe||&7e matalos muito fácil!|| ||&7%{knock-online}% jogando!||&eClique aqui para ir até a warp!" to close then run [make {_p} execute command "knock"]
		format slot 14 of {_p} with lava bucket named "&aLAVA CHALLENGE" with lore "&7Quer treinar seu refill? Este é||&7o desafio perfeito para você. Tente||&7sobreviver em um rio de lava.|| ||&7%{lava-online}% jogando!||&eClique aqui para ir até a warp!" to close then run [Make {_p} execute command "lava"]
		format slot 16 of {_p} with blaze rod named "&c1v1" with lore "||||&cEm breve!" to run [make {_p} execute command "som 1"]
	if {_type} is "shop":
		wait 4 tick
		if {_p} has permission "khkits.kits.*":
			set {kits-all.%{_p}%} to 23
		open chest with 3 row named "Loja" to {_p}
		play sound "CLICK" to {_p} with volume 0.5 and pitch 15.0
		wait 2 tick
		format slot 12 of {_p} with diamond sword named "&aKits" with lore "&7 Tenha vantagem extra ao usar||&7 nossos kits que possuem poderes||&7 especiais.|||| &fDesbloqueados: &c%{kits-all.%{_p}%}%/23 |||| &eClique aqui para ver!" to run [make {_p} execute command "kit buy main"]
		format slot 14 of {_p} with player head with custom nbt "{display:{Name:""&aGritos de Mortes""},SkullOwner:{Id:""b03562f3-2a20-4257-bb62-e040f552c297"",Properties:{textures:[{Value:""eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvMWYxYjg3NWRlNDljNTg3ZTNiNDAyM2NlMjRkNDcyZmYyNzU4M2ExZjA1NGYzN2U3M2ExMTU0YjViNTQ5OCJ9fX0=""}]}}}" with lore "&7 Gritos de mortes são sons que||&7 irão ser reproduzidos toda vez||&7 que você morrer.||||&f Desbloqueados: &c-/-||||&e Clique aqui para ver!" to run [make {_p} execute command "/som 1"]
	if {_type} is "kits":
		wait 3 tick
		open chest with 6 row named "Kits 1/2" to {_p}
		play sound "CLICK" to {_p} with volume 0.5 and pitch 15.0
		wait 2 tick
		format slot 10 of {_p} with stone sword named "&aNormal" with lore "||&7  Este é um kit espécial para quem||&7  é novo aqui no servidor.||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit normal"]
		format slot 11 of {_p} with diamond boots named "&aAnti Stomper" with lore "||&7  Ficou puto com os stompers?||&7  Com este kit você não levará||&7  dano por stomper!||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit antistomper"]
		format slot 12 of {_p} with bow named "&aArcher" with lore "||&7  Gosta de jogo de tiro? Então||&7  este kit é perfito para você.||&7  Com ele você ganhará um arco e||&7  flechas para atirar o quanto quiser!||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit archer"]
		format slot 13 of {_p} with sand named "&aCamel" with lore "||&7  Seja o rei do deserto! ao||&7  pisar no chão do deserto você||&7  receberá poderes para lhe ajudar!||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit Camel"]
		format slot 14 of {_p} with 11-disc named "&aDarkness" with lore "||&7  Com este kit você terá 30%% de||&7  chance de aplicar cegueira||&7  em seu inimigo.||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit darkness"]
		format slot 15 of {_p} with cactus named "&aEspinhos" with lore "||&7  Ao levar dano, você terá 25%%||&7  de chance de devolver o seu dano||&7  para seu inimigo.||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit espinhos"]
		format slot 16 of {_p} with fishing rod named "&aFisherman" with lore "||&7  Com este kit você poderá pescar||&7  seus inimigos e levá-los até você.||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit fish"]
		format slot 19 of {_p} with blaze powder named "&aFogaréu" with lore "||&7  Este kit te dará chances de||&7  aplicar fogo em seus inimigos||&7  e lhe dará poder de resistência!||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit fogareu"]
		format slot 20 of {_p} with player head with custom nbt "{display:{Name:""&aFrog""},SkullOwner:{Id:""6e0e69d8-ac58-40a1-baf8-68de9d85cff8"",Properties:{textures:[{Value:""eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvZDJjM2I5OGFkYTE5OTU3ZjhkODNhN2Q0MmZhZjgxYTI5MGZhZTdkMDhkYmY2YzFmODk5MmExYWRhNDRiMzEifX19""}]}}}" with lore "||&7  Torne-se um sapo com este kit! Ganhe||&7  super pulo  para usar contra os ||&7  seus oponentes e para pular obstáculos!||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit frog"]
		format slot 21 of {_p} with feather named "&aGalinha Voadora" with lore "||&7  Use este kit para fugir de||&7  lutas, quando ativado ele fará||&7  você voar por 4 segundos.||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit galinha"]
		format slot 22 of {_p} with diamond sword named "&aGuerreiro" with lore "||&7  Seja um guerreiro e mate todos||&7  os seus inimigos com esta espada||&7  poderosa!||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit guerreiro"]
		format slot 23 of {_p} with golden chestplate named "&aInvencível" with lore "||&7  Este kit te deixará invencível por||&7  quatro segundos após segurar||&7  o shift!||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit invencivel"]
		format slot 24 of {_p} with firework rocket named "&aKangaroo" with lore "||&7  Você pode usar este kit para ||&7  avançar ou fugir de lutas, ||&7  clicando com o botão direito no ||&7  foguete.||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit kangaroo"]
		format slot 25 of {_p} with blaze rod named "&aMonk" with lore "||&7  Ao usar este kit você irá atrapalhar||&7  o seu inimigo, bagunçando o inventário||&7  dele!||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit monk"]
		format slot 28 of {_p} with nether star named "&aNinja" with lore "||&7  Com este kit, você poderá se||&7  teleportar para o último jogador||&7  que você hitou apenas apertando||&7  shift.||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit ninja"]
		format slot 29 of {_p} with white glass pane named "&aPhanton" with lore "||&7  Ao usar este kit, você ficará||&7  completamente invisivel, por||&7  5 segundos.||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit phanton"]
		format slot 30 of {_p} with bowl named "&aQuick Dropper" with lore "||&7  Tem preguiça de ficar dropando||&7  potes? Com este kit, os seus||&7  potes serão dropados||&7  automaticamente!||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit quick"]
		format slot 31 of {_p} with soul sand named "&aSnail" with lore "||&7  Com este kit você terá 30%% de||&7  chance de aplicar lentidão||&7  em seu inimigo.||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit snail"]
		format slot 32 of {_p} with anvil named "&aStomper" with lore "||&7  Ao tomar dano de queda, você||&7  fará com que seus inimigos ao||&7  seu redor sinta a mesma dor.||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit stomper"]
		format slot 33 of {_p} with diamond chestplate named "&aTank" with lore "||&7  Este kit lhe dará um peitoral||&7  de diamante, porem você irá ficar||&7  mais lento do que o normal,||&7  pois ela é muito pesada.||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit tank"]
		format slot 34 of {_p} with wooden axe named "&aThor" with lore "||&7  Que tal ser o deus do Trovão? Com||&7  este kit você poderá liberar raios||&7  em seus oponentes com apenas um clique.||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit thor"]
		format slot 50 of {_p} with arrow named "&aPróxima página" to run [make {_p} execute command "kit page2"]
	if {_type} is "kits-2":
		wait 3 tick
		open chest with 6  row named "Kits 2/2" to {_p}
		play sound "CLICK" to {_p} with volume 0.5 and pitch 15.0
		wait 2 tick
		format slot 10 of {_p} with clock named "&aTime Lord" with lore "||&7  Com este kit você será poderá ||&7  literalmente parar o tempo, ele ||&7  deixará todos os jogadores paralisados||&7  por um certo tempo.||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit time"]
		format slot 11 of {_p} with spider eye named "&aViper" with lore "||&7  Ao atacar algum inimigo com este||&7  kit, você terá 20%% de chance de||&7  enfeitiça-lo com veneno.||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit viper"]
		format slot 12 of {_p} with slime named "&aZen" with lore "||&7  Ao usar este kit você irá se||&7  teletransportar para um jogador||&7  aleatorio do servidor.||||&a ▪ Clique para pegar este kit!  " to close then run [Make {_p} execute command "/kit zen"]
		format slot 48 of {_p} with arrow named "&aPágina anterior" to run [make {_p} execute command "kit page1"]
	if {_type} is "shop-kits":
		wait 4 tick
		open chest with 6  row named "Loja - Kits 1/1" to {_p}
		play sound "CLICK" to {_p} with volume 0.5 and pitch 15.0
		wait 2 tick
		getPrice()
		loop "antistomper" and "espinhos" and "stomper" and "fish" and "darkness" and "snail" and "ninja" and "guerreiro" and "camel" and "archer" and "kangaroo" and "frog" and "tank" and "thor" and "galinha" and "quick" and "invencivel" and "monk" and "time" and "fogareu" and "phanton":
			set {price-%loop-value%} to intSpliter("%{price-%loop-value%}%")
		format slot 10 of {_p} with diamond boots named "&aAnti Stomper" with lore "||&7  Ficou puto com os stompers?||&7  Com este kit você não levará||&7  dano por stomper!||||&f Preço: &6%{price-antistomper}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy antistomper"]
		format slot 11 of {_p} with bow named "&aArcher" with lore "||&7  Gosta de jogo de tiro? Então||&7  este kit é perfito para você.||&7  Com ele você ganhará um arco e||&7  flechas para atirar o quanto quiser!||||&f Preço: &6%{price-archer}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy archer"]
		format slot 12 of {_p} with sand named "&aCamel" with lore "||&7  Seja o rei do deserto! ao||&7  pisar no chão do deserto você||&7  receberá poderes para lhe ajudar!||||&f Preço: &6%{price-camel}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy camel"]
		format slot 13 of {_p} with 11-disc named "&aDarkness" with lore "||&7  Com este kit você terá 30%% de||&7  chance de aplicar cegueira||&7  em seu inimigo.||||&f Preço: &6%{price-darkness}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy darkness"]
		format slot 14 of {_p} with cactus named "&aEspinhos" with lore "||&7  Ao levar dano, você terá 25%%||&7  de chance de devolver o seu dano||&7  para seu inimigo.||||&f Preço: &6%{price-espinhos}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy espinhos"]
		format slot 15 of {_p} with fishing rod named "&aFisherman" with lore "||&7  Com este kit você poderá pescar||&7  seus inimigos e levá-los até você.||||&f Preço: &6%{price-fish}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy fish"]
		format slot 16 of {_p} with blaze powder named "&aFogaréu" with lore "||&7  Este kit te dará chances de||&7  aplicar fogo em seus inimigos||&7  e lhe dará poder de resistência!||||&f Preço: &6%{price-fogareu}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy fogareu"]
		format slot 19 of {_p} with player head with custom nbt "{display:{Name:""&aFrog""},SkullOwner:{Id:""6e0e69d8-ac58-40a1-baf8-68de9d85cff8"",Properties:{textures:[{Value:""eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvZDJjM2I5OGFkYTE5OTU3ZjhkODNhN2Q0MmZhZjgxYTI5MGZhZTdkMDhkYmY2YzFmODk5MmExYWRhNDRiMzEifX19""}]}}}" with lore "||&7  Torne-se um sapo com este kit! Ganhe||&7  super pulo  para usar contra os ||&7  seus oponentes e para pular obstáculos!||||&f Preço: &6%{price-frog}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy frog"]
		format slot 20 of {_p} with feather named "&aGalinha Voadora" with lore "||&7  Use este kit para fugir de||&7  lutas, quando ativado ele fará||&7  você voar por 4 segundos.||||&f Preço: &6%{price-galinha}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy galinha"]
		format slot 21 of {_p} with diamond sword named "&aGuerreiro" with lore "||&7  Seja um guerreiro e mate todos||&7  os seus inimigos com esta espada||&7  poderosa!||||&f Preço: &6%{price-guerreiro}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy guerreiro"]
		format slot 22 of {_p} with golden chestplate named "&aInvencível" with lore "||&7  Este kit te deixará invencível por||&7  quatro segundos após segurar||&7  o shift!||||&f Preço: &6%{price-invencivel}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy invencivel"]
		format slot 23 of {_p} with firework rocket named "&aKangaroo" with lore "||&7  Você pode usar este kit para ||&7  avançar ou fugir de lutas, ||&7  clicando com o botão direito no ||&7  foguete.||||&f Preço: &6%{price-kangaroo}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy kangaroo"]
		format slot 24 of {_p} with blaze rod named "&aMonk" with lore "||&7  Ao usar este kit você irá atrapalhar||&7  o seu inimigo, bagunçando o inventário||&7  dele!||||&f Preço: &6%{price-monk}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy monk"]
		format slot 25 of {_p} with nether star named "&aNinja" with lore "||&7  Com este kit, você poderá se||&7  teleportar para o último jogador||&7  que você hitou apenas apertando||&7  shift.||||&f Preço: &6%{price-ninja}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy ninja"]
		format slot 28 of {_p} with white glass pane named "&aPhanton" with lore "||&7  Ao usar este kit, você ficará||&7  completamente invisivel, por||&7  5 segundos.||||&f Preço: &6%{price-phanton}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy phanton"]
		format slot 29 of {_p} with bowl named "&aQuick Dropper" with lore "||&7  Tem preguiça de ficar dropando||&7  potes? Com este kit, os seus||&7  potes serão dropados||&7  automaticamente!||||&f Preço: &6%{price-quick}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy quick"]
		format slot 30 of {_p} with soul sand named "&aSnail" with lore "||&7  Com este kit você terá 30%% de||&7  chance de aplicar lentidão||&7  em seu inimigo.||||&f Preço: &6%{price-snail}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy snail"]
		format slot 31 of {_p} with anvil named "&aStomper" with lore "||&7  Ao tomar dano de queda, você||&7  fará com que seus inimigos ao||&7  seu redor sinta a mesma dor.||||&f Preço: &6%{price-stomper}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy stomper"]
		format slot 32 of {_p} with diamond chestplate named "&aTank" with lore "||&7  Este kit lhe dará um peitoral||&7  de diamante, porem você irá ficar||&7  mais lento do que o normal,||&7  pois ela é muito pesada.||||&f Preço: &6%{price-tank}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy tank"]
		format slot 33 of {_p} with wooden axe named "&aThor" with lore "||&7  Que tal ser o deus do Trovão? Com||&7  este kit você poderá liberar raios||&7  em seus oponentes com apenas um clique.||||&f Preço: &6%{price-thor}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy thor"]
		format slot 34 of {_p} with clock named "&aTime Lord" with lore "||&7  Com este kit você será poderá ||&7  literalmente parar o tempo, ele ||&7  deixará todos os jogadores paralisados||&7  por um certo tempo.||||&f Preço: &6%{price-time}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy time"]
		format slot 49 of {_p} with rose red named "&cVoltar" to run [make {_p} execute command "/kit buy shop"]
		format slot 50 of {_p} with arrow named "&aPróxima página" to run [make {_p} execute command "kit bpage2"]
	if {_type} is "shop-kits-2":
		wait 4 tick
		open chest with 6  row named "Loja - Kits 2/2" to {_p}
		play sound "CLICK" to {_p} with volume 0.5 and pitch 15.0
		wait 2 tick
		getPrice()
		loop "zen" and "viper":
			set {price-%loop-value%} to intSpliter("%{price-%loop-value%}%")
		format slot 10 of {_p} with spider eye named "&aViper" with lore "||&7  Ao atacar algum inimigo com este||&7  kit, você terá 20%% de chance de||&7  enfeitiça-lo com veneno.||||&f Preço: &6%{price-viper}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy viper"]
		format slot 11 of {_p} with slime named "&aZen" with lore "||&7  Ao usar este kit você irá se||&7  teletransportar para um jogador||&7  aleatorio do servidor.||||&f Preço: &6%{price-zen}% moedas||||&c ▪ Clique para comprar este kit!  " to close then run [Make {_p} execute command "/kit buy zen"]
		format slot 48 of {_p} with arrow named "&aPágina anterior" to run [make {_p} execute command "kit buy main"]
		format slot 49 of {_p} with rose red named "&cVoltar" to run [make {_p} execute command "/kit buy shop"]
function getPrice():
	set {price-guerreiro} to 7000
	set {price-camel} to 5000
	set {price-archer} to 6000
	set {price-kangaroo} to 10000
	set {price-frog} to 4000
	set {price-thor} to 8000
	set {price-tank} to 10000
	set {price-galinha} to 9000
	set {price-viper} to 11000
	set {price-invencivel} to 15000
	set {price-monk} to 13000
	set {price-time} to 15000
	set {price-fogareu} to 13000
	set {price-phanton} to 10000
	set {price-ninja} to 20000
	set {price-snail} to 18000
	set {price-darkness} to 18000
	set {price-fish} to 20000
	set {price-stomper} to 25000
	set {price-espinhos} to 23000
	set {price-antistomper} to 18000
	set {price-quick} to 15000
	set {price-zen} to 15000
function translate_file_codes(type: text, val1: text):
	replace all "&" in {%{_type}%::%{_val1}%} with "§"
	replace all "Ã§" in {%{_type}%::%{_val1}%} with "ç"
	replace all "Ã©" in {%{_type}%::%{_val1}%} with "é"
	replace all "Ã£" in {%{_type}%::%{_val1}%} with "ã"
	replace all "Ã¡" in {%{_type}%::%{_val1}%} with "á"
	replace all "Ã§" in {%{_type}%::%{_val1}%} with "ç"
	replace all "Ãª" in {%{_type}%::%{_val1}%} with "ê"
	replace all "Ãµ" in {%{_type}%::%{_val1}%} with "õ"
	replace all "Ã­" in {%{_type}%::%{_val1}%} with "í"
	replace all "Ã³" in {%{_type}%::%{_val1}%} with "ó"
	replace all "Ã " in {%{_type}%::%{_val1}%} with "à"
function translate_files_codes2(type: text, val1: text, val2: text):
	replace all "&" in {%{_type}%::%{_val1}%::%{_val2}%} with "§"
	replace all "{nl}" in {%{_type}%::%{_val1}%::%{_val2}%} with "||"
	replace all "Ã§" in {%{_type}%::%{_val1}%::%{_val2}%} with "ç"
	replace all "Ã©" in {%{_type}%::%{_val1}%::%{_val2}%} with "é"
	replace all "Ã£" in {%{_type}%::%{_val1}%::%{_val2}%} with "ã"
	replace all "Ã¡" in {%{_type}%::%{_val1}%::%{_val2}%} with "á"
	replace all "Ã§" in {%{_type}%::%{_val1}%::%{_val2}%} with "ç"
	replace all "Ãª" in {%{_type}%::%{_val1}%::%{_val2}%} with "ê"
	replace all "Ãµ" in {%{_type}%::%{_val1}%::%{_val2}%} with "õ"
	replace all "Ã­" in {%{_type}%::%{_val1}%::%{_val2}%} with "í"
	replace all "Ã³" in {%{_type}%::%{_val1}%::%{_val2}%} with "ó"
	replace all "Ã " in {%{_type}%::%{_val1}%::%{_val2}%} with "à"
function getSlot(text: text) :: text:
	set {_string::*} to {_text} split at " : "
	set {_slot} to {_string::1}
	replace all "slot=" in {_slot} with ""
	return "%{_slot}%"
function getItem(text: text) :: text:
	set {_string::*} to {_text} split at " : "
	set {_item} to {_string::2}
	replace all "item=" in {_item} with ""
	return "%{_item}%"
function getName(text: text) :: text:
	set {_string::*} to {_text} split at " : "
	set {_name} to {_string::3}
	replace all "named=" in {_name} with ""
	return "%{_name}%"
function getText(p: player, var: text) :: text:
	set {_kit-sel.%{_p}%} to yml value "%uuid of {_p}%.selected-kit" of file "{@playerc-file}"
	set {_coins.%{_p}%} to yml value "%uuid of {_p}%.coins.balance" of file "{@playerc-file}"
	set {_coins.%{_p}%} to intSpliter("%{_coins.%{_p}%}%")
	set {_deaths.%{_p}%} to yml value "%uuid of {_p}%.deaths" of file "{@playerc-file}"
	set {_kills.%{_p}%} to yml value "%uuid of {_p}%.kills" of file "{@playerc-file}"
	replace all "{now}" in {_var} with "%now%"
	replace all "{kit}" in {_var} with "%{_kit-sel.%{_p}%}%"
	replace all "{group}" in {_var} with "%{group.%{_p}%}%"
	replace all "{kills}" in {_var} with "%{_kills.%{_p}%}%"
	replace all "{deaths}" in {_var} with "%{_deaths.%{_p}%}%"
	replace all "{coins}" in {_var} with "%{_coins.%{_p}%}%"
	replace all "{name}" and "{player}" in {_var} with {_p}'s displayname
	replace all "{online}" in {_var} with "%number of all players%"
	replace all "&" in {_var} with "§"
	replace all "Ã§" in {_var} with "ç"
	replace all "Ã©" in {_var} with "é"
	replace all "Ã£" in {_var} with "ã"
	replace all "Ã¡" in {_var} with "á"
	replace all "Ã§" in {_var} with "ç"
	replace all "Ãª" in {_var} with "ê"
	replace all "Ãµ" in {_var} with "õ"
	replace all "Ã­" in {_var} with "í"
	replace all "Ã³" in {_var} with "ó"
	replace all "Ã " in {_var} with "à"
	return "%{_var}%"
function getTextrole(var: text) :: text:
	replace all "&" in {_var} with "§"
	replace all "Ã§" in {_var} with "ç"
	replace all "Ã©" in {_var} with "é"
	replace all "Ã£" in {_var} with "ã"
	replace all "Ã¡" in {_var} with "á"
	replace all "Ã§" in {_var} with "ç"
	replace all "Ãª" in {_var} with "ê"
	replace all "Ãµ" in {_var} with "õ"
	replace all "Ã­" in {_var} with "í"
	replace all "Ã³" in {_var} with "ó"
	replace all "Ã " in {_var} with "à"
	return "%{_var}%"
function khkits_setgroup(p: player):
	loop {groups::*}:
		if "%{node::%loop-value%.permission}%" is "none":
			set {rank.%{_p}%} to "%loop-value%"
			set {prefix.%{_p}%} to "%{node::%loop-value%.prefix}%"
			set {group.%{_p}%} to "%colored {node::%loop-value%.color}%%{node::%loop-value%.name}%"
			set {colored.%{_p}%} to "%colored {node::%loop-value%.color}%%{_p}%"
			set {display.%{_p}%} to "%{node::%loop-value%.prefix}%%{_p}%"
			set {position.%{_p}%} to "%{node::%loop-value%.position}%" parsed as int
		if {_p} has permission "%{node::%loop-value%.permission}%":
			set {rank.%{_p}%} to "%loop-value%"
			set {prefix.%{_p}%} to "%{node::%loop-value%.prefix}%"
			set {group.%{_p}%} to "%colored {node::%loop-value%.color}%%{node::%loop-value%.name}%"
			set {colored.%{_p}%} to "%colored {node::%loop-value%.color}%%{_p}%"
			set {display.%{_p}%} to "%{node::%loop-value%.prefix}%%{_p}%"
			set {position.%{_p}%} to "%{node::%loop-value%.position}%" parsed as int
		khkits_updateTag({_p})
function khkits_updateTag(p: player):
	if {prefix.%{_p}%} contains " ":
		make console execute command "nte player %{_p}% prefix %{prefix.%{_p}%}%%{node::%{rank.%{_p}%}%.color}%"
	else:
		make console execute command "nte player %{_p}% prefix %{prefix.%{_p}%}%"
	make console execute command "nte player %{_p}% priority %{position.%{_p}%}%"
#━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━#
# kits
function khkits_kits(p: player, kit: text):
	if {_kit} is not "zen" or "quick" or "antistomper" or "espinhos" or "stomper" or "fish" or "darkness" or "snail" or "ninja" or "camel" or "guerreiro" or "archer" or "frog" or "kangaroo" or "thor" or "tank" or "galinha" or "viper" or "invencivel" or "normal" or "phanton" or "fogareu" or "time" or "monk":
		send "&cEste kit não existe" to {_p}
	else:
		set {khkp::sel-kit} to yml value "options.messages.select-kit" of file "{@lang-file}"
		replace all "&" with "§" in {khkp::sel-kit}
		if {_kit} is set:
			set {_kitn} to {_kit}
			replace all "normal" in {_kitn} with "Normal"
			replace all "phanton" in {_kitn} with "Phanton"
			replace all "fogareu" in {_kitn} with "Fogaréu"
			replace all "time" in {_kitn} with "Time Lord"
			replace all "monk" in {_kitn} with "Monk"
			replace all "invencivel" in {_kitn} with "Invencivel"
			replace all "viper" in {_kitn} with "Viper"
			replace all "galinha" in {_kitn} with "Galinha Voadora"
			replace all "tank" in {_kitn} with "Tank"
			replace all "thor" in {_kitn} with "Thor"
			replace all "frog" in {_kitn} with "Frog"
			replace all "kangaroo" in {_kitn} with "Kangaroo"
			replace all "archer" in {_kitn} with "Archer"
			replace all "camel" in {_kitn} with "Camel"
			replace all "guerreiro" in {_kitn} with "Guerreiro"
			replace all "ninja" in {_kitn} with "Ninja"
			replace all "snail" in {_kitn} with "Snail"
			replace all "darkness" in {_kitn} with "Darkness"
			replace all "fish" in {_kitn} with "Fisherman"
			replace all "stomper" in {_kitn} with "Stomper"
			replace all "espinhos" in {_kitn} with "Espinhos"
			replace all "antistomper" in {_kitn} with "Anti Stomper"
			replace all "quick" in {_kitn} with "Quick Dropper"
			replace all "zen" in {_kitn} with "Zen"
			replace all "{kit}" with "%{_kitn}%" in {khkp::sel-kit}
			if {_kit} is "normal":
				set {_p}'s gamemode to adventure
				set {_p}'s fly mode to false
				khkits_join({_p}, "hotbar", "cleareffects")
				milk {_p}
				khkits_join({_p}, "hotbar", "cleareffects")
				khkits_join({_p}, "hotbar", "soups")
				set slot 0 of {_p} to stone sword named "&eEspada"
				equip {_p} with iron Chestplate named "&ePeitoral"
				set action bar of {_p} to "%{khkp::sel-kit}%"
				set yml value "%uuid of {_p}%.selected-kit" of file "{@playerc-file}" to "%{_kitn}%"
				play sound "ITEM PICKUP" to {_p} with volume 0.5 and pitch 15.0
			else:
				if {_p} has permission "khkits.kits.%{_kit}%":
					set {_p}'s gamemode to adventure
					set {_p}'s fly mode to false
					khkits_join({_p}, "hotbar", "cleareffects")
					milk {_p}
					khkits_join({_p}, "hotbar", "cleareffects")
					khkits_join({_p}, "hotbar", "soups")
					set slot 0 of {_p} to unbreakable stone sword named "&eEspada"
					equip {_p} with unbreakable iron Chestplate named "&ePeitoral"
					set action bar of {_p} to "%{khkp::sel-kit}%"
					set yml value "%uuid of {_p}%.selected-kit" of file "{@playerc-file}" to "%{_kitn}%"
					play sound "ITEM PICKUP" to {_p} with volume 0.5 and pitch 15.0
					set {sword-%{_p}%} to unbreakable stone sword
					if {_kit} is "guerreiro":
						set slot 0 of {_p} to unbreakable diamond sword of sharpness 1 named "&eEspada"
						set {sword-%{_p}%} to unbreakable diamond sword of sharpness 1
						equip {_p} with unbreakable leather Chestplate named "&ePeitoral"
						dye {_p}'s chestplate gray
					if {_kit} is "camel":
						set {Camel.%{_p}%} to true
					if {_kit} is "archer":
						set slot 8 of {_p} to unbreakable bow of infinity 1 named "&eArco"
						set slot 17 of {_p} to arrow named "&eMunição"
						equip {_p} with unbreakable gold Chestplate named "&ePeitoral"
					if {_kit} is "kangaroo":
						set slot 8 of {_p} to firework rocket named "&eKangaroo"
						set {Kangaro.%{_p}%} to true
					if {_kit} is "frog":
						set slot 0 of {_p} to unbreakable diamond sword named "&eEspada"
						set {sword-%{_p}%} to unbreakable diamond sword
						equip {_p} with player head with custom nbt "{display:{Name:""&eFrog""},SkullOwner:{Id:""6e0e69d8-ac58-40a1-baf8-68de9d85cff8"",Properties:{textures:[{Value:""eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvZDJjM2I5OGFkYTE5OTU3ZjhkODNhN2Q0MmZhZjgxYTI5MGZhZTdkMDhkYmY2YzFmODk5MmExYWRhNDRiMzEifX19""}]}}}"
						equip {_p} with unbreakable leather Chestplate named "&ePeitoral"
						dye {_p}'s chestplate green
						apply jump boost 3 to {_p} for 99999 seconds
					if {_kit} is "thor":
						set slot 8 of {_p} to unbreakable wooden axe named "&eThor"
						equip {_p} with player head with custom nbt "{display:{Name:""&eThor""},SkullOwner:{Id:""7529b65e-a551-4f43-90d3-7f38318953f6"",Properties:{textures:[{Value:""eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvMmE5ZjgzMzI5YTJlNDc1YTc1MzM1YjM5NDlhYTRkMDU0ZjlkZTQxM2JmYjI4YWE2MGRlMmU1MjU5ZWNhYWQxIn19fQ==""}]}}}"
						set {Thor.%{_p}%} to true
					if {_kit} is "tank":
						equip {_p} with unbreakable diamond Chestplate named "&ePeitoral"
						apply mining fatigue 1 to {_p} for 99999 seconds
						apply slowness 1 to {_p} for 99999 seconds
					if {_kit} is "galinha":
						set slot 8 of {_p} to feather named "&eGalinha Voadora"
						set {Fly.%{_p}%} to true
					if {_kit} is "viper":
						set {Viper.%{_p}%} to true
					if {_kit} is "invencivel":
						set {Invencivel.%{_p}%} to true
					if {_kit} is "monk":
						set slot 8 of {_p} to blaze rod named "&eMonk"
						set {Monk.%{_p}%} to true
					if {_kit} is "time":
						set slot 8 of {_p} to clock named "&eTime Lord"
						set {TimeLord.%{_p}%} to true
					if {_kit} is "fogareu":
						apply fire resistance 1 to {_p} for 99999 seconds
						set {fogareu.%{_p}%} to true
					if {_kit} is "phanton":
						set slot 8 of {_p} to white glass pane named "&ePhanton"
						set {phanton.%{_p}%} to true
					if {_kit} is "ninja":
						set {ninja.%{_p}%} to true
					if {_kit} is "snail":
						set {snail.%{_p}%} to true
					if {_kit} is "darkness":
						set {darkness.%{_p}%} to true
					if {_kit} is "fish":
						set slot 8 of {_p} to unbreakable fishing rod named "&eFisherman"
						set {fisherman.%{_p}%} to true
					if {_kit} is "stomper":
						set {stomper.%{_p}%} to true
					if {_kit} is "espinhos":
						set {espinhos.%{_p}%} to true
					if {_kit} is "antistomper":
						set {antistomper.%{_p}%} to true
					if {_kit} is "quick":
						set {quick.%{_p}%} to true
					if {_kit} is "zen":
						set {zen.%{_p}%} to true
						set slot 8 of {_p} to slime named "&eZen"
				else:
					send "&cVocê não comprou este kit." to {_p}
function khkits_kitsbuy(p: player, kit: text):
	set {_coins.%{_p}%} to yml value "%uuid of {_p}%.coins.balance" of file "{@playerc-file}"
	set {_coinslosted.%{_p}%} to yml value "%uuid of {_p}%.coins.losted" of file "{@playerc-file}"
	if {_kit} is set:
		set {_kitn} to {_kit}
		if {_kit} is not "zen" or "quick" or "antistomper" or "espinhos" or "stomper" or "fish" or "darkness" or "snail" or "ninja" or "camel" or "guerreiro" or "archer" or "frog" or "kangaroo" or "thor" or "tank" or "galinha" or "viper" or "invencivel" or "normal" or "phanton" or "fogareu" or "time" or "monk":
			send "&cEste kit não existe" to {_p}
		else:
			replace all "phanton" in {_kitn} with "Phanton"
			replace all "fogareu" in {_kitn} with "Fogaréu"
			replace all "time" in {_kitn} with "Time Lord"
			replace all "monk" in {_kitn} with "Monk"
			replace all "invencivel" in {_kitn} with "Invencivel"
			replace all "viper" in {_kitn} with "Viper"
			replace all "galinha" in {_kitn} with "Galinha Voadora"
			replace all "tank" in {_kitn} with "Tank"
			replace all "thor" in {_kitn} with "Thor"
			replace all "frog" in {_kitn} with "Frog"
			replace all "kangaroo" in {_kitn} with "Kangaroo"
			replace all "archer" in {_kitn} with "Archer"
			replace all "camel" in {_kitn} with "Camel"
			replace all "guerreiro" in {_kitn} with "Guerreiro"
			replace all "ninja" in {_kitn} with "Ninja"
			replace all "snail" in {_kitn} with "Snail"
			replace all "darkness" in {_kitn} with "Darkness"
			replace all "fish" in {_kitn} with "Fisherman"
			replace all "stomper" in {_kitn} with "Stomper"
			replace all "espinhos" in {_kitn} with "Espinhos"
			replace all "antistomper" in {_kitn} with "Anti Stomper"
			replace all "quick" in {_kitn} with "Quick Dropper"
			replace all "zen" in {_kitn} with "Zen"
			if {_p} does not have permission "khkits.kit.%{_kit}%" or "khkits.kits.*" or "khkits.*":
				if {_coins.%{_p}%} is greater or equal to {price-%{_kit}%}:
					send "&aVocê comprou o kit &n%{_kitn}%&a!" to {_p}
					execute console command "/pex user %{_p}% add khkits.kit.%{_kit}%"
					play sound "ITEM PICKUP" to {_p} with volume 0.5 and pitch 15.0
					remove {price-%{_kit}%} from {_coins.%{_p}%}
					add {price-%{_kit}%} to {_coinslosted.%{_p}%}
					add 1 to {kits-all.%{_p}%}
					set yml value "%uuid of {_p}%.coins.balance" of file "{@playerc-file}" to "%{_coins.%{_p}%}%" parsed as int
					set yml value "%uuid of {_p}%.coins.losted" of file "{@playerc-file}" to "%{_coinslosted.%{_p}%}%" parsed as int
				else:
					send "&cVocê não tem moedas o suficiente." to {_p}
			else:
				send "&cVocê já possui este kit." to {_p}
command /kit [<text>] [<text>]:
	trigger:
		if arg 1 is "buy":
			if arg 2 is "main":
				khkits_menus(player, "shop-kits")
			else:
				if arg 2 is "shop":
					khkits_menus(player, "shop")
				else:
					khkits_kitsbuy(player, arg 2)
		else:
			if arg 1 is "bpage2":
				khkits_menus(player, "shop-kits-2")
			else:
				if arg 1 is "page1":
					khkits_menus(player, "kits")
				else:
					if arg 1 is "page2":
						khkits_menus(player, "kits-2")
					else:
						set {_kit-sel.%player%} to yml value "%uuid of player%.selected-kit" of file "{@players-file}"
						if {_kit-sel.%player%} is not "%{khkp::warpfps}%" or "%{khkp::warpknock}%" or "%{khkp::warplava}%":
							if arg 1 is set:
								khkits_kits(player, arg 1)
						else:
							send "&cVocê não pode usar este comando aqui!" to player
#━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━#
# join e outros
on join:
	khkits_setgroup(player)
	if file "{@players-file}" doesn't exist:
		create file "{@players-file}"
		set yml value "%uuid of player%.name" of file "{@players-file}" to "%player%"
		set yml value "%uuid of player%.kills" of file "{@players-file}" to 0
		set yml value "%uuid of player%.deaths" of file "{@players-file}" to 0
		set yml value "%uuid of player%.selected-kit" of file "{@players-file}" to "%{khkp::kitnone}%"
		set yml value "%uuid of player%.coins.balance" of file "{@players-file}" to 0
		set yml value "%uuid of player%.coins.losted" of file "{@players-file}" to 0
		set yml value "%uuid of player%.coins.earned" of file "{@players-file}" to 0
		set yml value "%uuid of player%.others.left-clicks" of file "{@players-file}" to 0
		set yml value "%uuid of player%.others.right-clicks" of file "{@players-file}" to 0
		set yml value "%uuid of player%.others.chat-messages" of file "{@players-file}" to 0
		set yml value "%uuid of player%.others.soups-eated" of file "{@players-file}" to 0
		set yml value "%uuid of player%.others.attack-with-swords" of file "{@players-file}" to 0
		set yml value "%uuid of player%.login.first" of file "{@players-file}" to "%now%"
	set yml value "%uuid of player%.selected-kit" of file "{@players-file}" to "%{khkp::kitnone}%" 
	set yml value "%uuid of player%.login.last" of file "{@players-file}" to "%now%"
	if {kits-all.%player%} is not set:
		set {kits-all.%player%} to 0
	set {warp-fps.%player%} to false
	set {warp-knock.%player%} to false
	set {warp-lava.%player%} to false
	delete {build.%player%}
	set {scoreboard.%player%} to true
	khkits_join(player, "update", "tablist")
	khkits_join(player, "hotbar", "cleareffects")
	showScoreboard(player)
	set player's gamemode to adventure
	teleport player to {khkits-spawn}
	khkits_join(player, "hotbar", "itens")
	if player has permission "khkits.voar" or "khkits.*":
		set player's fly mode to true
	if player has permission "khkits.viplogin" or "khkits.*":
		set {khkp::playerjoin} to yml value "account.lobby.join-broadcast" of file "{@lang-file}"
		replace all "{display}" with coloured {display.%player%} in {khkp::playerjoin}
		set join message to colored {khkp::playerjoin}
	else:
		set join message to ""
on quit:
	set quit message to ""
	delete {scoreboard.%player%}
on server list ping:
	if {khkp::motd.enabled} is true:
		set motd to "%colored {khkp::motd.header}%%nl%%colored {khkp::motd.footer}%"
#━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━#
# warps
on command:
	if command is "spawn" or "fps" or "lava" or "knock":
		cancel event
		make player execute command "warps %command%"
command /warps [<text>] [<text>]:
	trigger:
		if arg 1 is not set:
			make player execute command "warps help"
		if arg 1 is "help" or "ajuda":
			if player has permission "khkits.knock" or "khkits.lava" or "khkits.fps" or "khkits.spawn" or "khkits.*":
				send "%nl%  &eAjuda - Warps%nl% "
				send "&3 /warps spawn &f- &7Configura a warp spawn%nl%&3 /warps fps &f- &7Configura a warp fps%nl%&3 /warps lava &f- &7Configura a warp lava%nl%&3 /warps knock &f- &7Configura a warp knock%nl%&3 /warps 1v1 &f- &7EM BREVE%nl% "
		loop "spawn" and "fps" and "lava" and "knock":
			if arg 1 is "%loop-value%":
				if arg 2 is not set:
					if {khkits-%loop-value%} is set:
						delete {build.%player%}
						khkits_join(player, "hotbar", "cleareffects")
						set player's gamemode to adventure
						set player's fly mode to false
						extinguish player
						loop 3 and 2 and 1:
							set {khkp::warps-teleport} to yml value "options.messages.warps-teleport.delay" of file "{@lang-file}"
							replace all "{time}" with "%loop-value-2%" in {khkp::warps-teleport}
							set action bar of player to colored {khkp::warps-teleport}
							play sound "ORB PICKUP" to player with volume 0.5 and pitch 15.0
							wait 1 second
						play "LEVEL_UP" to player at volume 0.5
						set action bar of player to colored {khkp::warps-sucess}
						set {warp-knock.%player%} and {warp-lava.%player%} and {warp-fps.%player%} to false
						if loop-value is "fps":
							set {warp-fps.%player%} to true
							khkits_join(player, "hotbar", "soups")
							set slot 0 of player to unbreakable iron sword named "&eEspada"
							equip player with unbreakable iron Chestplate named "&ePeitoral"
							teleport player to {khkits-fps}
							if file "{@players-file}" exists:
								set yml value "%uuid of player%.selected-kit" of file "{@players-file}" to "%{khkp::warpfps}%" 
							stop
						if loop-value is "lava":
							set {warp-lava.%player%} to true
							khkits_join(player, "hotbar", "soups")
							teleport player to {khkits-lava}
							if file "{@players-file}" exists:
								set yml value "%uuid of player%.selected-kit" of file "{@players-file}" to "%{khkp::warplava}%" 
							stop
						if loop-value is "knock":
							set {warp-knock.%player%} to true
							khkits_join(player, "hotbar", "soups")
							set slot 0 of player to stick of knockback 5 and sharpness 2 named "&eBastão"
							equip player with iron Chestplate named "&ePeitoral"
							teleport player to {khkits-knock}
							if file "{@players-file}" exists:
								set yml value "%uuid of player%.selected-kit" of file "{@players-file}" to "%{khkp::warpknock}%" 
							stop
						if loop-value is "spawn":
							teleport player to {khkits-spawn}
							khkits_join(player, "hotbar", "itens")
							if player has permission "khkits.voar" or "khkits.*":
								set player's fly mode to true
							if file "{@players-file}" exists:
								set yml value "%uuid of player%.selected-kit" of file "{@players-file}" to "%{khkp::kitnone}%" 
							stop
					else:
						if player has permission "khkits.%loop-value%" or "khkits.*":
							send "%nl%  &eAjuda - Warps %loop-value%%nl% "
							send "&3 /warps %loop-value% set&f- &7Define a warp %loop-value%%nl% "
				if arg 2 is "set":
					if player has permission "khkits.%loop-value%" or "khkits.*":
						set {khkits-%loop-value%} to location of player
						set action bar of player to "&aO &l%loop-value% &afoi definido com sucesso."
#━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━#
# economia
command /moedas [<text>] [<offlineplayer>] [<int>]:
	aliases: /money, /cash, /coins
	trigger:
		set {_coins.%player%} to yml value "%uuid of player%.coins.balance" of file "{@players-file}"
		if arg 1 is not set:
			set {_coins.%player%} to intSpliter("%{_coins.%player%}%")
			send "&fSeu saldo: &6%{_coins.%player%}%"
		if arg 1 is "add" or "dar" or "give":
			if player has permission "khkits.moedas.add" or "khkits.*":
				if arg 3 is set:
					if offlineplayer-arg is not online:
						send "&cEste jogador não esta online no momento."
						stop
					set {_args-file} to "{@players-file}"
					replace all "%player%" in {_args-file} with "%offlineplayer-arg%"
					set {_coins.%offlineplayer-arg%} to yml value "%uuid of offlineplayer-arg%.coins.balance" of file "%{_args-file}%"
					set {_cearned.%offlineplayer-arg%} to yml value "%uuid of offlineplayer-arg%.coins.earned" of file "%{_args-file}%"
					add arg-3 to {_coins.%offlineplayer-arg%}
					add arg-3 to {_cearned.%offlineplayer-arg%}
					set yml value "%uuid of offlineplayer-arg%.coins.balance" of file "%{_args-file}%" to "%{_coins.%offlineplayer-arg%}%" parsed as int
					set yml value "%uuid of offlineplayer-arg%.coins.earned" of file "%{_args-file}%" to "%{_cearned.%offlineplayer-arg%}%" parsed as int
					if arg-2 is not "%player%":
						send "&e * Você adicionou &n%arg-3%&e moedas para &n%offlineplayer-arg%&e." to player
					send "&e * Você recebeu &n%arg-3%&e moedas." to offlineplayer-arg
				else:
					send "&cUtilize: /moedas add <jogador> <quantidade>" to player
		if arg 1 is "remover" or "del" or "remove":
			if player has permission "khkits.moedas.remove" or "khkits.*":
				if arg 3 is set:
					if offlineplayer-arg is not online:
						send "&cEste jogador não esta online no momento."
						stop
					set {_args-file} to "{@players-file}"
					replace all "%player%" in {_args-file} with "%offlineplayer-arg%"
					set {_coins.%offlineplayer-arg%} to yml value "%uuid of offlineplayer-arg%.coins.balance" of file "%{_args-file}%"
					set {_closted.%offlineplayer-arg%} to yml value "%uuid of offlineplayer-arg%.coins.losted" of file "%{_args-file}%"
					if {_coins.%offlineplayer-arg%} > 0:
						remove arg-3 from {_coins.%offlineplayer-arg%}
						add arg-3 to {_closted.%offlineplayer-arg%}
						if {_coins.%offlineplayer-arg%} < 0:
							set {_coins.%offlineplayer-arg%} to 0
						set yml value "%uuid of offlineplayer-arg%.coins.balance" of file "%{_args-file}%" to "%{_coins.%offlineplayer-arg%}%" parsed as int
						set yml value "%uuid of offlineplayer-arg%.coins.losted" of file "%{_args-file}%" to "%{_closted.%offlineplayer-arg%}%" parsed as int
						if arg-2 is not "%player%":
							send "&e * Você removeu &n%arg 3%&e moedas de &n%offlineplayer-arg%&e." to player
						send "&e * Foi removido &n%arg 3%&e moedas da sua conta." to offlineplayer-arg
					else:
						send "&cEste jogador não tem dinheiro para remover." to player
				else:
					send "&cUtilize: /moedas remove <jogador> <quantidade>" to player
#━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━#
# sign
on sign change:
	line 1 is "[khkits]" or "[khkp]":
		line 2 is "refill" or "refil":
			set line 1 to colored {khkp::signs.1}
			set line 2 to colored {khkp::signs.2}
			set line 3 to colored {khkp::signs.3}
			set line 4 to colored {khkp::signs.4}
	line 1 is "[khkits]" or "[khkp]":
		line 2 is "recraft" or "crafts":
			set line 1 to colored {khkp::signr.1}
			set line 2 to colored {khkp::signr.2}
			set line 3 to colored {khkp::signr.3}
			set line 4 to colored {khkp::signr.4}
on rightclick on sign:
	if line 1 of clicked block is colored {khkp::signs.1}:
		if line 2 of clicked block is colored {khkp::signs.2}:
			if line 3 of clicked block is colored {khkp::signs.3}:
				if line 4 of clicked block is colored {khkp::signs.4}:
					open chest with 6 row named colored {khkp::refil-title} to player
					wait 1 tick
					set {_soupslot} to 0
					loop 54 times:
						set slot {_soupslot} of player's current inventory to mushroom stew named colored {khkp::soup-name}
						add 1 to {_soupslot}
	if line 1 of clicked block is colored {khkp::signr.1}:
		if line 2 of clicked block is colored {khkp::signr.2}:
			if line 3 of clicked block is colored {khkp::signr.3}:
				if line 4 of clicked block is colored {khkp::signr.4}:
					open chest with 3 row named colored {khkp::recraft-title} to player
					wait 1 tick
					set {_r1} to 0
					loop 3 times:
						loop 9 times:
							set slot {_r1} of player's current inventory to 64 red mushroom named colored {khkp::rmush-name}
							add 1 to {_r1}
						loop 9 times:
							set slot {_r1} of player's current inventory to 64 brown mushroom named colored {khkp::bmush-name}
							add 1 to {_r1}
						loop 9 times:
							set slot {_r1} of player's current inventory to 64 bowl named colored {khkp::pot-name}
							add 1 to {_r1}
#━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━#
# chat
on chat:
	set {_chatmsg.%player%} to yml value "%uuid of player%.others.chat-messages" of file "{@players-file}"
	add 1 to {_chatmsg.%player%}
	set yml value "%uuid of player%.others.chat-messages" of file "{@players-file}" to "%{_chatmsg.%player%}%" parsed as int
	set {_waitchat} to difference between {chat.%player%.delay} and now
	if {_waitchat} is less than 3 seconds:
		cancel event
		set {playerx.tempo} to "%difference between 3 seconds and {_waitchat}%"
		replace all " seconds" and " second" with "" in {playerx.tempo}
		set {khkp::chat-delay} to yml value "options.chat.delay" from file "{@lang-file}"
		replace all "{time}" in {khkp::chat-delay} with {playerx.tempo}
		send colored {khkp::chat-delay} to player
	set {khkp::chat-format} to yml value "options.chat.format" from file "{@lang-file}"
	if player has permission "khkits.chat.color" or "khkits.*":
		set {_color} to yml value "options.chat.color.custom" from file "{@lang-file}"
		replace all "{message}" in {khkp::chat-format} with colored message
	else:
		set {_color} to yml value "options.chat.color.default" from file "{@lang-file}"
		set {chat.%player%.delay} to now
		replace all "{message}" in {khkp::chat-format} with message
	replace all "{player}" in {khkp::chat-format} with player's displayname
	replace all "{color}" in {khkp::chat-format} with colored {_color}
	replace all "{prefix}" in {khkp::chat-format} with colored {prefix.%player%}
	set chat format to {khkp::chat-format}
#━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━#
# stats
command /stats [<offlineplayer>]:
	trigger:
		if arg 1 is not set:
			make player execute command "stats %player%"
			stop
		if arg 1 is set:
			if offlineplayer-arg is online:
				wait 3 tick
				set {args-stats} to "{@players-file}"
				replace all "%player%" with "%offlineplayer-arg%" in {args-stats}
				open chest with 4 row named "&8%{colored.%offlineplayer-arg%}%&8 - Estatísticas" to player
				play sound "CLICK" to player with volume 0.5 and pitch 15.0
				wait 1 tick
				set {_p} to offlineplayer-arg
				set {_gdsword.%offlineplayer-arg%} to yml value "%uuid of offlineplayer-arg%.others.attack-with-swords" of file "%{args-stats}%"
				set {_coins.%offlineplayer-arg%} to yml value "%uuid of offlineplayer-arg%.coins.balance" of file "%{args-stats}%"
				set {_coinsearned.%offlineplayer-arg%} to yml value "%uuid of offlineplayer-arg%.coins.earned" of file "%{args-stats}%"
				set {_coinslosted.%offlineplayer-arg%} to yml value "%uuid of offlineplayer-arg%.coins.losted" of file "%{args-stats}%"
				set {_deaths.%offlineplayer-arg%} to yml value "%uuid of offlineplayer-arg%.deaths" of file "%{args-stats}%"
				set {_kills.%offlineplayer-arg%} to yml value "%uuid of offlineplayer-arg%.kills" of file "%{args-stats}%"
				set {_soupseat.%offlineplayer-arg%} to yml value "%uuid of offlineplayer-arg%.others.soups-eated" of file "%{args-stats}%"
				set {_chatmsg.%offlineplayer-arg%} to yml value "%uuid of offlineplayer-arg%.others.chat-messages" of file "%{args-stats}%"
				set {_leftclicks.%offlineplayer-arg%} to yml value "%uuid of offlineplayer-arg%.others.left-clicks" of file "%{args-stats}%"
				set {_rightclicks.%offlineplayer-arg%} to yml value "%uuid of offlineplayer-arg%.others.right-clicks" of file "%{args-stats}%"
				set {_flogin.%offlineplayer-arg%} to yml value "%uuid of offlineplayer-arg%.login.first" of file "%{args-stats}%"
				set {_llogin.%offlineplayer-arg%} to yml value "%uuid of offlineplayer-arg%.login.last" of file "%{args-stats}%"
				format slot 13 of player with skull of {_p} named "&7%{colored.%offlineplayer-arg%}%" with lore "" to run [make player execute command "som 2"]
				format slot 20 of player with diamond sword named "&AOfênsivas" with lore "&7Abates: &a%{_kills.%offlineplayer-arg%}%||&7Golpes de Espada: &a%{_gdsword.%offlineplayer-arg%}%||&7Projéteis Atirados: &aem breve||&7Projéteis Acertados: &aem breve||" to run [make player execute command "som 2"]
				format slot 21 of player with mob head item named "&aDefênsivas" with lore "&7Mortes: &a%{_deaths.%offlineplayer-arg%}%||" to run [make player execute command "som 2"]
				format slot 22 of player with gold ingot named "&aBanco" with lore "&7Moedas Atuais: &a%{_coins.%offlineplayer-arg%}%||&7Moedas Perdidas: &a%{_coinslosted.%offlineplayer-arg%}%||&7Moedas Totais: &a%{_coinsearned.%offlineplayer-arg%}%" to run [make player execute command "som 2"]
				format slot 23 of player with item frame named "&aDados" with lore "&7Cargo: &7%{group.%offlineplayer-arg%}%||&7Cadastrado em: &f%{_flogin.%offlineplayer-arg%}%||&7Último login: &f%{_llogin.%offlineplayer-arg%}%" to run [make player execute command "som 2"]
				format slot 24 of player with flower pot item named "&aOutros" with lore "&7Cliques Esquerdos: &a%{_leftclicks.%offlineplayer-arg%}%||&7Cliques Direitos: &a%{_rightclicks.%offlineplayer-arg%}%||&7Mensagens no Chat: &a%{_chatmsg.%offlineplayer-arg%}%||&7Sopas Tomadas: &a%{_soupseat.%offlineplayer-arg%}%" to run [make player execute command "som 2"]
#━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━#
# clicks
on click:
	if player's tool is any sword:
		set {_gdsword.%player%} to yml value "%uuid of player%.others.attack-with-swords" of file "{@players-file}"
		add 1 to {_gdsword.%player%}
		set yml value "%uuid of player%.others.attack-with-swords" of file "{@players-file}" to "%{_gdsword.%player%}%" parsed as int
on rightclick:
	set {_rightclicks.%player%} to yml value "%uuid of player%.others.right-clicks" of file "{@players-file}"
	add 1 to {_rightclicks.%player%}
	set yml value "%uuid of player%.others.right-clicks" of file "{@players-file}" to "%{_rightclicks.%player%}%" parsed as int
	if name of player's tool is getName({khkp::hotbar-shop}):
		khkits_menus(player, "shop")
	if name of player's tool is getName({khkp::hotbar-kits}):
		khkits_menus(player, "kits")
	if name of player's tool is getName({khkp::hotbar-warps}):
		khkits_menus(player, "warps")
	if player's tool is a mushroom soup:
		if health of player is less than 9.5:
			cancel event
			set {_soupseat.%player%} to yml value "%uuid of player%.others.soups-eated" of file "{@players-file}"
			add 1 to {_soupseat.%player%}
			set yml value "%uuid of player%.others.soups-eated" of file "{@players-file}" to "%{_soupseat.%player%}%" parsed as int
			set {life.%player%} to 2.1 or 2.2 or 2.3 or 2.4 or 2.5 or 2.6 or 2.7 or 2.8 or 2.9 or 3.1 or 3.2 or 3.3 or 3.4 or 3.5 or 3.6 or 3.7 or 3.8 or 3.9
			set action bar of player to "&7+%{life.%player%}% &4&l❤"
			set the health of player to the health of player + {life.%player%}
			delete {life.%player%}
			set {_slot} to selected hotbar slot of player
			if {quick.%player%} is not true:
				set slot {_slot} of player to bowl named colored {khkp::pot-name}
			else:
				set slot {_slot} of player to air
				drop bowl at player
				play "ITEM_PICKUP" to player at volume 0.5
			play "BURP" to player at volume 0.5
on leftclick:
	set {_leftclicks.%player%} to yml value "%uuid of player%.others.left-clicks" of file "{@players-file}"
	add 1 to {_leftclicks.%player%}
	set yml value "%uuid of player%.others.left-clicks" of file "{@players-file}" to "%{_leftclicks.%player%}%" parsed as int
on inventory click:
	set {_kit-sel.%player%} to yml value "%uuid of player%.selected-kit" of file "{@players-file}"
	if {_kit-sel.%player%} is "%{khkp::kitnone}%":
		if {build.%player%} is not true:
			cancel event
#━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━#
# proteção
on walk on carpet:
	if block down is a sponge:
		push player upwards at speed 3.0
		push player forward at speed 2.5
on death of a player:
	clear drops
on xp spawn:
	cancel event
at 11:01:
	set time to 10:00
on weather change to rain or thunder:
	cancel event
on hunger meter change:
	cancel event
on hunger bar change:
	cancel event
on drop:
	if event-item is not a bowl:
		if {build.%player%} is not true:
			cancel event
	else:
		loop all dropped items:
			wait 2 seconds
			delete loop-value
on pickup:
	if {build.%player%} is not true:
		cancel event
on build:
	if {build.%player%} is not true:
		cancel event
on place:
	if {build.%player%} is not true:
		cancel event
on break:
	if {build.%player%} is not true:
		cancel event
on right-click on beacon or lever or any door or any anvil or furnaces or any chest or crafting table or enchantment table or brewing stand or bed or dispenser or dropper or any trapdoor or any fence gate:
	if {build.%player%} is not true:
		cancel event
command /build [<text>]:
	trigger:
		if player has permission "khkits.build" or "khkits.*":
			if {build.%player%} is not true:
				set {build.%player%} to true
				set action bar of player to colored {khkp::buildon}
				set player's gamemode to creative
			else:
				delete {build.%player%}
				set player's gamemode to adventure
				set action bar of player to colored {khkp::buildoff}
				if player has permission "khkits.voar" or "khkits.*":
					set {_kit-sel.%player%} to yml value "%uuid of player%.selected-kit" of file "{@players-file}"
					if {_kit-sel.%player%} is "%{khkp::kitnone}%":
						set player's fly mode to true
		else:
			send colored {khkp::noperm} to player
command /voar:
	aliases: /fly
	trigger:
		if player has permission "khkits.voar" or "khkits.*":
			set {_kit-sel.%player%} to yml value "%uuid of player%.selected-kit" of file "{@players-file}"
			if {_kit-sel.%player%} is "%{khkp::kitnone}%":
				if player's fly mode is true:
					play sound "ITEM PICKUP" to player with volume 0.5 and pitch 15.0
					set action bar of player to colored {khkp::flyoff}
					set player's fly mode to false
				else:
					play sound "ITEM PICKUP" to player with volume 0.5 and pitch 15.0
					set action bar of player to colored {khkp::flyon}
					set player's fly mode to true
			else:
				send "&cVocê não pode usar este comando aqui!" to player
		else:
			send colored {khkp::noperm} to player
command /som [<text>]:
	trigger:
		if arg 1 is "1":
			play sound "ENDERMAN_TELEPORT" to player with volume 0.5 and pitch 5.0
		if arg 1 is "2":
			play sound "ITEM PICKUP" to player with volume 0.5 and pitch 15.0
#━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━#
# damage
sub "respawn":
	set {_p} to parameter 1
	set {_p2} to parameter 2	
	set {_uuid} to uuid of {_p}
	set {_uuid2} to uuid of {_p2}
	khkits_join({_p}, "hotbar", "cleareffects")
	delete {build.%{_p}%}
	set {_p}'s gamemode to adventure
	extinguish {_p}
	if {warp-fps.%{_p}%} is true:
		set yml value "%uuid of {_p}%.selected-kit" of file "{@playerc-file}" to "%{khkp::warpfps}%" 
		set slot 0 of {_p} to unbreakable iron sword named "&eEspada"
		equip {_p} with unbreakable iron Chestplate named "&ePeitoral"
		khkits_join({_p}, "hotbar", "soups")
		teleport {_p} to {khkits-fps}
		stop
	if {warp-knock.%{_p}%} is true:
		set yml value "%uuid of {_p}%.selected-kit" of file "{@playerc-file}" to "%{khkp::warpknock}%" 
		set slot 0 of {_p} to stick of knockback 5 and sharpness 2 named "&eBastão"
		equip {_p} with iron Chestplate named "&ePeitoral"
		khkits_join({_p}, "hotbar", "soups")
		teleport {_p} to {khkits-knock}
		stop
	if {warp-lava.%{_p}%} is true:
		set yml value "%uuid of {_p}%.selected-kit" of file "{@playerc-file}" to "%{khkp::warplava}%" 
		khkits_join({_p}, "hotbar", "soups")
		teleport {_p} to {khkits-lava}
		stop
	teleport {_p} to {khkits-spawn}
	khkits_join({_p}, "hotbar", "itens")
	set yml value "%uuid of {_p}%.selected-kit" of file "{@playerc-file}" to "%{khkp::kitnone}%" 
	if {_p} has permission "khkits.voar" or "khkits.*":
		set {_p}'s fly mode to true
on damage of a player:
	if attacker is set:
		if attacker is a player:
			set {_kit-sel.%attacker%} to yml value "%uuid of attacker%.selected-kit" of file "plugins/KhKits/players/%attacker%.yml"
			if {_kit-sel.%attacker%} is "%{khkp::kitnone}%":
				cancel event
	if victim is a player:
		set {_kit-sel.%victim%} to yml value "%uuid of victim%.selected-kit" of file "{@playerv-file}"
		damage cause is fall:
			cancel event
		damage cause is void:			
			invoke "respawn" from {_victim} and attacker
		if {_kit-sel.%victim%} is "%{khkp::kitnone}%":
			cancel event
		if attacker is not victim:
			if "%damage cause%" is "projectile":
				if projectile is arrow:
					if victim is not dead:
						set {_kit-sel.%victim%} to yml value "%uuid of victim%.selected-kit" of file "{@playerv-file}"
						if {_kit-sel.%victim%} is not "%{khkp::kitnone}%":
							send "%{colored.%victim%}%&e está com &c%victim's health%&e de HP!" to attacker				
	if damage is greater than or equal to health of victim:
		if victim is a player:
			set {_kit-sel.%victim%} to yml value "%uuid of victim%.selected-kit" of file "{@playerv-file}"
			set {_coins.%victim%} to yml value "%uuid of victim%.coins.balance" of file "{@playerv-file}"
			set {_closted.%victim%} to yml value "%uuid of victim%.coins.losted" of file "{@playerv-file}"
			set {_deaths.%victim%} to yml value "%uuid of victim%.deaths" of file "{@playerv-file}"
			if {_kit-sel.%victim%} is "%{khkp::kitnone}%":
				cancel event
				stop
			set {_victim} to victim
			set {_attacker} to attacker			
			set {lasthit.%{_victim}%} to {_attacker}
			if {_kit-sel.%victim%} is "%{khkp::warplava}%":
				remove 15 from {_coins.%victim%}
				add 15 to {_closted.%victim%}
			else:
				remove 25 from {_coins.%victim%}
				add 25 to {_closted.%victim%}
			add 1 to {_deaths.%victim%}
			if {_coins.%victim%} < 0:
				set {_coins.%victim%} to 0
			if {_closted.%victim%} < 0:
				set {_closted.%victim%} to 0
			set yml value "%uuid of victim%.coins.balance" of file "{@playerv-file}" to "%{_coins.%victim%}%" parsed as int
			set yml value "%uuid of victim%.coins.losted" of file "{@playerv-file}" to "%{_closted.%victim%}%" parsed as int
			set yml value "%uuid of victim%.deaths" of file "{@playerv-file}" to "%{_deaths.%victim%}%" parsed as int
			if {_kit-sel.%victim%} is "%{khkp::warplava}%":
				set action bar of victim to "&c- 15 moedas"	
			else:
				set action bar of victim to "&c- 25 moedas"					
			invoke "respawn" from {_victim} and attacker
			cancel event
			create a fake explosion at the victim
			if attacker is a player:
				delete {ninja-victim.%attacker%}
				set {_cearned.%attacker%} to yml value "%uuid of attacker%.coins.earned" of file "plugins/KhKits/players/%attacker%.yml"
				set {_coins.%attacker%} to yml value "%uuid of attacker%.coins.balance" of file "plugins/KhKits/players/%attacker%.yml"
				set {_kills.%attacker%} to yml value "%uuid of attacker%.kills" of file "plugins/KhKits/players/%attacker%.yml"
				if {_kit-sel.%victim%} is "%{khkp::warpknock}%":
					add 1 to {_kills.%attacker%}
					add 50 to {_coins.%attacker%}
					add 50 to {_cearned.%attacker%}
					set yml value "%uuid of attacker%.kills" of file "plugins/KhKits/players/%attacker%.yml" to "%{_kills.%attacker%}%" parsed as int
					set yml value "%uuid of attacker%.coins.balance" of file "plugins/KhKits/players/%attacker%.yml" to "%{_coins.%attacker%}%" parsed as int
					set yml value "%uuid of attacker%.coins.earned" of file "plugins/KhKits/players/%attacker%.yml" to "%{_cearned.%attacker%}%" parsed as int
					set action bar of attacker to "&6+ 50 moedas"
					loop all players:
						set {_kit-sel.%loop-player%} to yml value "%uuid of loop-player%.selected-kit" of file "plugins/KhKits/players/%loop-player%.yml"
						if {_kit-sel.%loop-player%} is "%{khkp::warpknock}%":
							send "&b ▪ &7%{colored.%victim%}%&7 foi abatido por %{colored.%attacker%}%&7." to loop-player
				if {_kit-sel.%victim%} is "%{khkp::warpfps}%":
					add 1 to {_kills.%attacker%}
					add 50 to {_coins.%attacker%}
					add 50 to {_cearned.%attacker%}
					set yml value "%uuid of attacker%.kills" of file "plugins/KhKits/players/%attacker%.yml" to "%{_kills.%attacker%}%" parsed as int
					set yml value "%uuid of attacker%.coins.balance" of file "plugins/KhKits/players/%attacker%.yml" to "%{_coins.%attacker%}%" parsed as int
					set yml value "%uuid of attacker%.coins.earned" of file "plugins/KhKits/players/%attacker%.yml" to "%{_cearned.%attacker%}%" parsed as int
					set action bar of attacker to "&6+ 50 moedas"
					loop all players:
						set {_kit-sel.%loop-player%} to yml value "%uuid of loop-player%.selected-kit" of file "plugins/KhKits/players/%loop-player%.yml"
						if {_kit-sel.%loop-player%} is "%{khkp::warpfps}%":
							send "&e ▪ &7%{colored.%victim%}%&7 foi abatido por %{colored.%attacker%}%&7." to loop-player
				if {_kit-sel.%victim%} is "%{khkp::warplava}%":
					add 1 to {_kills.%attacker%}
					add 25 to {_coins.%attacker%}
					add 25 to {_cearned.%attacker%}
					set yml value "%uuid of attacker%.kills" of file "plugins/KhKits/players/%attacker%.yml" to "%{_kills.%attacker%}%" parsed as int
					set yml value "%uuid of attacker%.coins.balance" of file "plugins/KhKits/players/%attacker%.yml" to "%{_coins.%attacker%}%" parsed as int
					set yml value "%uuid of attacker%.coins.earned" of file "plugins/KhKits/players/%attacker%.yml" to "%{_cearned.%attacker%}%" parsed as int
					set action bar of attacker to "&6+ 25 moedas"
					loop all players:
						set {_kit-sel.%loop-player%} to yml value "%uuid of loop-player%.selected-kit" of file "plugins/KhKits/players/%loop-player%.yml"
						if {_kit-sel.%loop-player%} is "%{khkp::warplava}%":
							send "&c ▪ &7%{colored.%victim%}%&7 foi abatido por %{colored.%attacker%}%&7." to loop-player
				if {_kit-sel.%victim%} is "%{khkp::warplava}%":
					add 1 to {_kills.%attacker%}
					add 25 to {_coins.%attacker%}
					add 25 to {_cearned.%attacker%}
					set yml value "%uuid of attacker%.kills" of file "plugins/KhKits/players/%attacker%.yml" to "%{_kills.%attacker%}%" parsed as int
					set yml value "%uuid of attacker%.coins.balance" of file "plugins/KhKits/players/%attacker%.yml" to "%{_coins.%attacker%}%" parsed as int
					set yml value "%uuid of attacker%.coins.earned" of file "plugins/KhKits/players/%attacker%.yml" to "%{_cearned.%attacker%}%" parsed as int
					set action bar of attacker to "&6+ 25 moedas"
					loop all players:
						set {_kit-sel.%loop-player%} to yml value "%uuid of loop-player%.selected-kit" of file "plugins/KhKits/players/%loop-player%.yml"
						if {_kit-sel.%loop-player%} is "%{khkp::warplava}%":
							send "&c ▪ &7%{colored.%victim%}%&7 foi abatido por %{colored.%attacker%}%&7." to loop-player
				if {_kit-sel.%victim%} is not "%{khkp::warplava}%" or "%{khkp::warpknock}%" or "%{khkp::warpfps}%":
					add 1 to {_kills.%attacker%}
					add 50 to {_coins.%attacker%}
					add 50 to {_cearned.%attacker%}
					set yml value "%uuid of attacker%.kills" of file "plugins/KhKits/players/%attacker%.yml" to "%{_kills.%attacker%}%" parsed as int
					set yml value "%uuid of attacker%.coins.balance" of file "plugins/KhKits/players/%attacker%.yml" to "%{_coins.%attacker%}%" parsed as int
					set yml value "%uuid of attacker%.coins.earned" of file "plugins/KhKits/players/%attacker%.yml" to "%{_cearned.%attacker%}%" parsed as int
					set action bar of attacker to "&6+ 50 moedas"
					loop all players:
						set {_kit-sel.%loop-player%} to yml value "%uuid of loop-player%.selected-kit" of file "plugins/KhKits/players/%loop-player%.yml"
						if {_kit-sel.%loop-player%} is not "%{khkp::warplava}%" or "%{khkp::warpknock}%" or "%{khkp::warpfps}%":
							send "&6 ▪ &7%{colored.%victim%}%&7 foi abatido por %{colored.%attacker%}%&7." to loop-player
			else:
				if {_kit-sel.%victim%} is "%{khkp::warplava}%":
					loop all players:
						set {_kit-sel.%loop-player%} to yml value "%uuid of loop-player%.selected-kit" of file "plugins/KhKits/players/%loop-player%.yml"
						if {_kit-sel.%loop-player%} is "%{khkp::warplava}%":
							send "&c ▪ &7%{colored.%victim%}%&7 morreu." to loop-player
#━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━#
# habilidades dos kits
on walk on sand or sandstone:
	set {_kit-sel.%player%} to yml value "%uuid of player%.selected-kit" of file "{@players-file}"
	set {khkp::perk-enabled} to yml value "options.messages.enable-kit-skill" of file "{@lang-file}"
	replace all "{kit}" with {_kit-sel.%player%} in {khkp::perk-enabled}
	if {_kit-sel.%player%} is "Camel":
		remove speed from player
		remove regeneration from player
		apply speed 1 to player for 3 seconds
		apply regeneration 1 to player for 3 seconds
		set action bar of player to colored {khkp::perk-enabled}
on rightclick:
	set {_kit-sel.%player%} to yml value "%uuid of player%.selected-kit" of file "{@players-file}"
	set {khkp::perk-enabled} to yml value "options.messages.enable-kit-skill" of file "{@lang-file}"
	replace all "{kit}" with {_kit-sel.%player%} in {khkp::perk-enabled}
	if name of player's tool is "&eKangaroo":
		if {_kit-sel.%player%} is "Kangaroo":
			cancel event 
			if block under player is not air:
				push player upward at speed 1
				push player forward at speed 1
				set action bar of player to colored {khkp::perk-enabled}
	if name of player's tool is "&eThor":
		if {_kit-sel.%player%} is "Thor":
			set {_waitthor} to difference between {thor.%player%.delay} and now
			if {_waitthor} is less than 30 seconds:
				set {thor.tempo} to "%difference between 30 seconds and {_waitthor}%"
				replace all " seconds" with "s" in {thor.tempo}
				replace all " second" with "s" in {thor.tempo}
				set action bar of player to "&cAguarde &e%{thor.tempo}% &cpara usar este kit novamente."
			else:
				set {thor.%player%.delay} to now
				strike lightning at the targeted block
				set action bar of player to colored {khkp::perk-enabled}
	if name of player's tool is "&ePhanton":
		if {_kit-sel.%player%} is "Phanton":
			set {_waitphanton} to difference between {phanton.%player%.delay} and now
			if {_waitphanton} is less than 20 seconds:
				set {phanton.tempo} to "%difference between 20 seconds and {_waitphanton}%"
				replace all " seconds" with "s" in {phanton.tempo}
				replace all " second" with "s" in {phanton.tempo}
				set action bar of player to "&cAguarde &e%{phanton.tempo}% &cpara usar este kit novamente."
			else:
				set {phanton.%player%.delay} to now
				set action bar of player to colored {khkp::perk-enabled}
				loop all players:
					hide player to loop-player
				wait 5 seconds
				loop all players:
					reveal player to loop-player
	if name of player's tool is "&eZen":
		if {_kit-sel.%player%} is "Zen":
			set {_waitzen} to difference between {zen.%player%.delay} and now
			if {_waitzen} is less than 15 seconds:
				set {zen.tempo} to "%difference between 15 seconds and {_waitzen}%"
				replace all " seconds" with "s" in {zen.tempo}
				replace all " second" with "s" in {zen.tempo}
				set action bar of player to "&cAguarde &e%{zen.tempo}% &cpara usar este kit novamente."
			else:
				set {zen.%player%.delay} to now
				set action bar of player to colored {khkp::perk-enabled}
				set {all-players::*} to all players
				remove player from {all-players::*}
				loop all players:
					set {_kit-sel.%loop-player%} to yml value "%uuid of loop-player%.selected-kit" of file "plugins/KhKits/players/%loop-player%.yml"
					if {_kit-sel.%loop-player%} is "%{khkp::warpfps}%" or "%{khkp::warpknock}%" or "%{khkp::warplava}%" or "%{khkp::kitnone}%":
						remove loop-player from {all-players::*}
				set {teleporter.%player%} to random player out of {all-players::*}
				teleport player to {teleporter.%player%}
				delete {all-players::*} and {teleporter.%player%}
				play sound "ENDERMAN_TELEPORT" to player with volume 0.5 and pitch 5.0
	if name of player's tool is "&eGalinha Voadora":
		if {_kit-sel.%player%} is "Galinha Voadora":
			set {_waitfly} to difference between {fly.%player%.delay} and now
			if {_waitfly} is less than 30 seconds:
				set {fly.tempo} to "%difference between 30 seconds and {_waitfly}%"
				replace all " seconds" with "s" in {fly.tempo}
				replace all " second" with "s" in {fly.tempo}
				set action bar of player to "&cAguarde &e%{fly.tempo}% &cpara usar este kit novamente."
			else:
				set {fly.%player%.delay} to now
				set player's fly mode to true
				Broadcast "%nl%&a * %{colored.%player%}%&a esta usando o kit Galinha Voadora.%nl% "
				set action bar of player to colored {khkp::perk-enabled}
				wait 5 seconds
				set player's fly mode to false
	if name of player's tool is "&eTime Lord":
		if {_kit-sel.%player%} is "Time Lord":
			set {_waittlord} to difference between {tlord.%player%.delay} and now
			if {_waittlord} is less than 20 seconds:
				set {tlord.tempo} to "%difference between 20 seconds and {_waittlord}%"
				replace all " seconds" with "s" in {tlord.tempo}
				replace all " second" with "s" in {tlord.tempo}
				set action bar of player to "&cAguarde &e%{tlord.tempo}% &cpara usar este kit novamente."
			else:
				set {tlord.%player%.delay} to now
				set action bar of player to colored {khkp::perk-enabled}
				set {_playert::*} to all players
				loop all players:
					set {_kit-sel.%loop-player%} to yml value "%uuid of loop-player%.selected-kit" of file "plugins/KhKits/players/%loop-player%.yml"
					if {_kit-sel.%loop-player%} is "%{khkp::warpfps}%" or "%{khkp::warpknock}%" or "%{khkp::warplava}%" or "%{khkp::kitnone}%":
						remove loop-player from {_playert::*}
				remove player from {_playert::*}
				remove slowness from {_playert::*}
				remove jump boost from {_playert::*}
				wait 1 tick
				apply slowness 250 to {_playert::*} for 10 seconds
				apply jump boost 250 to {_playert::*} for 10 seconds
				wait 10 seconds
				loop all players:
					if {_kit-sel.%loop-player%} is "Tank":
						remove slowness from loop-player
						remove jump boost from loop-player
						apply mining fatigue 1 to loop-player for 99999 seconds
						apply slowness 1 to loop-player for 99999 seconds
on rightclick holding a blaze rod on entity:
	set {_kit-sel.%player%} to yml value "%uuid of player%.selected-kit" of file "{@players-file}"
	set {khkp::perk-enabled} to yml value "options.messages.enable-kit-skill" of file "{@lang-file}"
	replace all "{kit}" with {_kit-sel.%player%} in {khkp::perk-enabled}
	if name of player's tool is "&eMonk":
		if {_kit-sel.%player%} is "Monk":
			set {_waitmonk} to difference between {monk.%player%.delay} and now
			if {_waitmonk} is less than 15 seconds:
				set {monk.tempo} to "%difference between 15 seconds and {_waitmonk}%"
				replace all " seconds" with "s" in {monk.tempo}
				replace all " second" with "s" in {monk.tempo}
				set action bar of player to "&cAguarde &e%{monk.tempo}% &cpara usar este kit novamente."
			else:
				set {monk.%player%.delay} to now
				set action bar of player to colored {khkp::perk-enabled}
				set {_slotmonk} to a random integer between 0 and 35
				if {_slotmonk} is 8 or 13 or 14 or 15 or 17:
					set {_slotmonk} to a random integer between 18 and 35
				remove any sword from the entity
				set slot {_slotmonk} of entity to {sword-%entity%} named "&eEspada"
				give entity 1 soup named colored {khkp::soup-name}
on damage:
	victim is a player
	set {_kit-sel.%victim%} to yml value "%uuid of victim%.selected-kit" of file "{@playerv-file}"
	if damage cause is void:
		if {_kit-sel.%victim%} is not "%{khkp::warpknock}%":
			khkits_join(victim, "hotbar", "cleareffects")
			delete {build.%victim%}
			set victim's gamemode to adventure
			extinguish victim
			if {_kit-sel.%victim%} is "%{khkp::warpfps}%":
				set yml value "%uuid of victim%.selected-kit" of file "{@playerv-file}" to "%{khkp::warpfps}%" 
				set slot 0 of victim to unbreakable iron sword named "&eEspada"
				equip victim with unbreakable iron Chestplate named "&ePeitoral"
				khkits_join(victim, "hotbar", "soups")
				teleport victim to {khkits-fps}
				stop
			if {_kit-sel.%victim%} is "%{khkp::warpknock}%":
				set yml value "%uuid of victim%.selected-kit" of file "{@playerv-file}" to "%{khkp::warpknock}%" 
				set slot 0 of victim to stick of knockback 5 and sharpness 2 named "&eBastão"
				equip victim with iron Chestplate named "&ePeitoral"
				khkits_join(victim, "hotbar", "soups")
				teleport victim to {khkits-knock}
				stop
			if {_kit-sel.%victim%} is "%{khkp::warplava}%":
				set yml value "%uuid of victim%.selected-kit" of file "{@playerv-file}" to "%{khkp::warplava}%" 
				khkits_join(victim, "hotbar", "soups")
				teleport victim to {khkits-lava}
				stop
			teleport victim to {khkits-spawn}
			khkits_join(victim, "hotbar", "itens")
			set yml value "%uuid of victim%.selected-kit" of file "{@playerv-file}" to "%{khkp::kitnone}%" 
		else:
			set victim's health to 2
	if damage cause is fall:
		cancel event
		if {_kit-sel.%victim%} is "Stomper":
			set {_players::*} to all players in radius 5 of victim
			loop all players:
				set {_kit-sel.%loop-player%} to yml value "%uuid of loop-player%.selected-kit" of file "plugins/KhKits/players/%loop-player%.yml"
				if {_kit-sel.%loop-player%} is "%{khkp::warpfps}%" or "%{khkp::warpknock}%" or "%{khkp::warplava}%" or "%{khkp::kitnone}%":
					remove loop-player from {_players::*}
				if {antistomper.%loop-player%} is true:
					remove loop-player from {_players::*}
			remove victim from {_players::*}
			loop {_players::*}:
				if loop-value is sneaking:
					remove loop-value from {_players::*}
			if size of {_players::*} is not 0:
				loop {_players::*}:
					set {damage.%victim%} to damage*2
					damage loop-value by {damage.%victim%} hearts
	if {_kit-sel.%victim%} is not "%{khkp::warpfps}%" or "%{khkp::warpknock}%" or "%{khkp::warplava}%" or "%{khkp::kitnone}%":
		set {khkp::perk-enabled} to yml value "options.messages.enable-kit-skill" of file "{@lang-file}"
		replace all "{kit}" with {_kit-sel.%attacker%} in {khkp::perk-enabled}
		if {_kit-sel.%victim%} is "Espinhos":
			chance of 25%:
				set {damage.%victim%} to damage/2
				damage attacker by {damage.%victim%} hearts
		if {ninja.%attacker%} is true:
			delete {ninja-time.%attacker%} and {ninja-victim.%attacker%}
			set {ninja-victim.%attacker%} to "%victim%"
			set {ninja-time.%attacker%} to 60
		if {_kit-sel.%attacker%} is "Fogaréu":
			chance of 35%:
				set fire to victim
				set action bar of attacker to colored {khkp::perk-enabled}
		if {_kit-sel.%attacker%} is "Snail":
			chance of 30%:
				remove slowness from victim
				apply slowness 3 to victim for 5 seconds
				set action bar of attacker to colored {khkp::perk-enabled}
		if {_kit-sel.%attacker%} is "Darkness":
			chance of 30%:
				remove slowness from victim
				apply blindness 10 to victim for 5 seconds
				set action bar of attacker to colored {khkp::perk-enabled}
		if {inv.effect.%victim%} is true:
			cancel event
			set action bar of attacker to "%{colored.%victim%}%&a esta usando o kit Invencível."
		if {_kit-sel.%attacker%} is "Viper":
			chance of 20%:
				attacker is a player
				apply poison 2 to victim for 3 seconds
				set action bar of attacker to colored {khkp::perk-enabled}
		if {_kit-sel.%attacker%} is "Fisherman":
			if name of attacker's tool is "&eFisherman":
				delete {target.%attacker%}
				set {target.%attacker%} to "%victim%" parsed as player
on fishing:
	set {_kit-sel.%player%} to yml value "%uuid of player%.selected-kit" of file "{@players-file}"
	if {_kit-sel.%player%} is "Fisherman":
		teleport {target.%player%} to player
		delete {target.%player%}
on sneak toggle:
	player is not sneaking
	set {_kit-sel.%player%} to yml value "%uuid of player%.selected-kit" of file "{@players-file}"
	set {khkp::perk-enabled} to yml value "options.messages.enable-kit-skill" of file "{@lang-file}"
	replace all "{kit}" with {_kit-sel.%player%} in {khkp::perk-enabled}
	if {_kit-sel.%player%} is "Ninja":
		if {ninja-victim.%player%} is not set:
			set action bar of player to "&cVocê não bateu em nenhum jogador nos ultimos segundos"
		else:
			#replace all "{kit}" with "Ninja" in {khkp::perk-enabled}
			set {_waitninja} to difference between {ninja.%player%.delay} and now
			if {_waitninja} is less than 15 seconds:
				set {ninja.tempo} to "%difference between 15 seconds and {_waitninja}%"
				replace all " seconds" with "s" in {ninja.tempo}
				replace all " second" with "s" in {ninja.tempo}
				set action bar of player to "&cAguarde &e%{ninja.tempo}% &cpara usar este kit novamente."
			else:
				set {ninja.%player%.delay} to now
				teleport player to location of {ninja-victim.%player%} parsed as player
				set action bar of player to coloured {khkp::perk-enabled}
				loop 60 times:
					if {ninja-time.%player%} is 0:
						delete {ninja-victim.%player%} and {ninja-time.%player%}
					else:
						remove 1 from {ninja-time.%player%}
						wait 1 second
	if {_kit-sel.%player%} is "Invencivel":
		#replace all "{kit}" with "Invencivel" in {khkp::perk-enabled}
		set {_waitinv} to difference between {inv.%player%.delay} and now
		if {_waitinv} is less than 20 seconds:
			set {inv.tempo} to "%difference between 20 seconds and {_waitinv}%"
			replace all " seconds" with "s" in {inv.tempo}
			replace all " second" with "s" in {inv.tempo}
			set action bar of player to "&cAguarde &e%{inv.tempo}% &cpara usar este kit novamente."
		else:
			set {inv.%player%.delay} to now
			set {inv.effect.%player%} to true
			set action bar of player to colored {khkp::perk-enabled}
			wait 4 seconds
			delete {inv.effect.%player%}
#━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━#
# comando do skript
command /khkits [<text>]:
	trigger:
		if arg 1 is not "help" or "reload":
			make player execute command "khkits help"
		if arg 1 is "help":
			if player has permission "khkits.*":
				send "%nl%  &eAjuda - KhKits 1/1%nl% "
				send "&3 /khkits atualizar &f- &7Atualiza o skript"
				send "&3 /khkits reload &f- &7Recarrega os arquivos%nl% "
			else:
				send "&6[KhKits] &7criado por &a&n<url:https://youtube.com/kickholse>Kick Holse<reset>&7, &a&n<url:>uOnyle<reset>&7 e &a&n<url:>Kiwi<reset>&7."
		if arg-1 is "atualizar":
			if {khkp::updater} is true:
				set action bar of player to "&aIniciando processo de atualização..."
				set {_updater} to 0
				wait 1 second
				loop 100 times:
					add 1.1 to {_updater}
					wait 1 tick
					if {_updater} > 100:
						set {_updater} to 100	
					set action bar of player to "&aAtualizando, isso pode demorar... &8(%{_updater}%%%)"
					if {_updater} = 100:
						set {_sk} to script name
						delete file "plugins/Skript/scripts/%{_sk}%.sk"
						download file from "https://raw.githubusercontent.com/okiwizn/skripts/master/KiwiSkyWars" to file "plugins/Skript/scripts/KiwiSkyWars.sk"
						#make console execute "sk reload KiwiSkyWars"
						reload script "%{_sk}%"
						set action bar of player to "&aO skript foi atualizado com sucesso."
						stop
			else:
				send "&cNão há atualizações do skript no momento."
		if arg 1 is "reload":
			if player has permission "khkits.reload":
				set action bar of player to "&e ▪ Verificando arquivos..."
				play sound "ORB PICKUP" to player with volume 0.5 and pitch 15.0
				wait 1 seconds
				if file "{@config-file}" doesn't exist:
					set action bar of player to "&e ▪ Criando config.yml..."
					play sound "ITEM PICKUP" to player with volume 0.5 and pitch 15.0
					create file "{@config-file}"
					khkits_setvalue("config-create")
					wait 1 second
				if file "{@lang-file}" doesn't exist:
					set action bar of player to "&e ▪ Criando lang.yml..."
					play sound "ITEM PICKUP" to player with volume 0.5 and pitch 15.0
					create file "{@lang-file}"
					khkits_setvalue("lang-create")
					wait 1 second
				if file "{@groups-file}" doesn't exist:
					set action bar of player to "&e ▪ Criando groups.yml..."
					play sound "ITEM PICKUP" to player with volume 0.5 and pitch 15.0
					create file "{@groups-file}"
					khkits_setvalue("groups-create")
					wait 1 second
				set action bar of player to "&e ▪ Carregando arquivos..."
				play sound "ORB PICKUP" to player with volume 0.5 and pitch 15.0
				wait 1 second
				khkits_setvalue("setvalue")
				set action bar of player to "&a ▪ Arquivos carregados com sucesso!"
				play sound "LEVEL UP" to player with volume 5.0 and pitch 15.0
#━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━#
# yml
function khkits_setvalue(do: text):
	set {_&} to "&"
	set {_¨} to "a"
	if {_do} is "setvalue":
		delete {khkp::*}
		set {khkp::checkversion} to yml value "check-update" of file "{@config-file}"
		set {khkp::hotbar-warps} to yml value "account.lobby.hotbar.warps" from file "{@lang-file}"
		set {khkp::hotbar-kits} to yml value "account.lobby.hotbar.kits" from file "{@lang-file}"
		set {khkp::hotbar-shop} to yml value "account.lobby.hotbar.shop" from file "{@lang-file}"
		set {khkp::noperm} to yml value "options.messages.no-perm" of file "{@lang-file}"
		set {khkp::motd.enabled} to yml value "options.motd.enabled" of file "{@lang-file}"
		set {khkp::motd.header} to yml value "options.motd.header" of file "{@lang-file}"
		set {khkp::motd.footer} to yml value "options.motd.footer" of file "{@lang-file}"
		set {khkp::tab.enabled} to yml value "options.tablist.enabled" of file "{@lang-file}"
		set {khkp::tab.header} to yml value "options.tablist.header" of file "{@lang-file}"
		set {khkp::tab.footer} to yml value "options.tablist.footer" of file "{@lang-file}"
		set {khkp::warps-sucess} to yml value "options.messages.warps-teleport.sucess" of file "{@lang-file}"
		set {khkp::flyon} to yml value "options.messages.fly.enabled" of file "{@lang-file}"
		set {khkp::flyoff} to yml value "options.messages.fly.disabled" of file "{@lang-file}"
		set {khkp::buildon} to yml value "options.messages.build.enabled" of file "{@lang-file}"
		set {khkp::buildoff} to yml value "options.messages.build.disabled" of file "{@lang-file}"
		set {khkp::signr.1} to yml value "options.signs.recraft.line-1" of file "{@lang-file}"
		set {khkp::signr.2} to yml value "options.signs.recraft.line-2" of file "{@lang-file}"
		set {khkp::signr.3} to yml value "options.signs.recraft.line-3" of file "{@lang-file}"
		set {khkp::signr.4} to yml value "options.signs.recraft.line-4" of file "{@lang-file}"
		set {khkp::recraft-title} to yml value "options.signs.recraft.title" of file "{@lang-file}"
		set {khkp::rmush-name} to yml value "options.signs.recraft.items.red-mushroom" of file "{@lang-file}"
		set {khkp::bmush-name} to yml value "options.signs.recraft.items.brown-mushroom" of file "{@lang-file}"
		set {khkp::pot-name} to yml value "options.signs.recraft.items.bowl" of file "{@lang-file}"
		set {khkp::signs.1} to yml value "options.signs.refill.line-1" of file "{@lang-file}"
		set {khkp::signs.2} to yml value "options.signs.refill.line-2" of file "{@lang-file}"
		set {khkp::signs.3} to yml value "options.signs.refill.line-3" of file "{@lang-file}"
		set {khkp::signs.4} to yml value "options.signs.refill.line-4" of file "{@lang-file}"
		set {khkp::refil-title} to yml value "options.signs.refill.title" of file "{@lang-file}"
		set {khkp::soup-name} to yml value "options.signs.refill.items.soup" of file "{@lang-file}"
		set {khkp::kitnone} to yml value "options.kit.none" of file "{@lang-file}"
		set {khkp::warpfps} to yml value "options.kit.fps" of file "{@lang-file}"
		set {khkp::warpknock} to yml value "options.kit.knockback" of file "{@lang-file}"
		set {khkp::warplava} to yml value "options.kit.lava-challenge" of file "{@lang-file}"
		loop "hotbar-warps" and "hotbar-kits" and "hotbar-shop":
			translate_file_codes("khkp", "%loop-value%")
		set {score.title} to yaml value "scoreboard.title" from file "{@lang-file}"
		set {_temp::*} to yaml list "scoreboard.lines" from file "{@lang-file}"
		set {score.lines} to 0
		set {_empty} to 0
		delete {score.line::*}
		set {_line} to amount of {_temp::*}
		loop {_temp::*}:
			add 1 to {score.lines}
			if "%loop-value%" is "":
				set {score.line::%{_line}%} to "&%{_empty}%"
				if {_empty} < 9:
					add 1 to {_empty}
			else:
				set {score.line::%{_line}%} to "%loop-value%"
			remove 1 from {_line}
		delete {groups::*}
		set {_temp::*} to skutil yaml nodes with keys "groups" from file "{@groups-file}"
		loop {_temp::*}:
			set {_group} to loop-value
			replace all ".name" and ".color" and ".prefix" and ".permission" in {_group} with ""
			if {groups::*} contains "%{_group}%":
				remove "%{_group}%" from {groups::*}
			add "%{_group}%" to {groups::*}
		set {_pos} to size of {groups::*}
		delete {node::*}
		loop {groups::*}:
			set {_group} to loop-value
			set {_name} to yaml value "groups.%{_group}%.name" from file "{@groups-file}"
			set {node::%{_group}%.color} to yaml value "groups.%{_group}%.color" from file "{@groups-file}"
			set {_prefix} to yaml value "groups.%{_group}%.prefix" from file "{@groups-file}"
			set {node::%{_group}%.position} to {_pos}
			set {node::%{_group}%.permission} to yaml value "groups.%{_group}%.permission" from file "{@groups-file}"
			set {node::%{_group}%.name} to getTextrole({_name})
			set {node::%{_group}%.prefix} to getTextrole({_prefix})
			remove 1 from {_pos}
	if {_do} is "lang-create":
		set yaml value "scoreboard.title" from file "{@lang-file}" to "%{_&}%%{_¨}%%{_&}%lKIT PVP"
		add "" to yaml list "scoreboard.lines" from file "{@lang-file}"
		add " Kit: %{_&}%7{kit}" to yaml list "scoreboard.lines" from file "{@lang-file}"
		add " Grupo: %{_&}%a{group}" to yaml list "scoreboard.lines" from file "{@lang-file}"
		add "" to yaml list "scoreboard.lines" from file "{@lang-file}"
		add "  Abates: %{_&}%a{kills}" to yaml list "scoreboard.lines" from file "{@lang-file}"
		add "  Mortes: %{_&}%a{deaths}" to yaml list "scoreboard.lines" from file "{@lang-file}"
		add "" to yaml list "scoreboard.lines" from file "{@lang-file}"
		add " Moedas: %{_&}%6{coins}" to yaml list "scoreboard.lines" from file "{@lang-file}"
		add " Online: %{_&}%a{online}" to yaml list "scoreboard.lines" from file "{@lang-file}"
		add "" to yaml list "scoreboard.lines" from file "{@lang-file}"
		add "%{_&}%ewww.kldev.tk" to yaml list "scoreboard.lines" from file "{@lang-file}"
		set yml value "account.lobby.join-broadcast" of file "{@lang-file}" to "%{_&}%7{display} %{_&}%6entrou neste lobby!"
		set yml value "account.lobby.hotbar.warps" from file "{@lang-file}" to "slot=3 : item=COMPASS : named=%{_&}%aWarps %{_&}%7(Clique direito)"
		set yml value "account.lobby.hotbar.kits" from file "{@lang-file}" to "slot=4 : item=NETHER_STAR : named=%{_&}%aKits %{_&}%7(Clique direito)"
		set yml value "account.lobby.hotbar.shop" from file "{@lang-file}" to "slot=5 : item=EMERALD : named=%{_&}%aLoja %{_&}%7(Clique direito)"
		set yml value "options.chat.color.default" from file "{@lang-file}" to "%{_&}%7"
		set yml value "options.chat.color.custom" from file "{@lang-file}" to "%{_&}%f"
		set yml value "options.chat.format" from file "{@lang-file}" to "{prefix}{player}{color}: {message}"
		set yml value "options.chat.delay" from file "{@lang-file}" to "%{_&}%cAguarde {time}s para utilizar o chat novamente!"
		set yml value "options.motd.enabled" of file "{@lang-file}" to true
		set yml value "options.motd.header" of file "{@lang-file}" to "%{_&}%2%{_&}%lKLDEV%{_&}%2.tk %{_&}%7[1.7 - 1.16]"
		set yml value "options.motd.footer" of file "{@lang-file}" to "%{_&}%e%{_&}%nKit PvP%{_&}%e aberto para VIPs!"
		set yml value "options.tablist.enabled" of file "{@lang-file}" to true
		set yml value "options.tablist.header" of file "{@lang-file}" to "\n%{_&}%2%{_&}%lKLDEV\n    %{_&}%fjogar.kldev.tk\n "
		set yml value "options.tablist.footer" of file "{@lang-file}" to " \n%{_&}%aDiscord: \n\n         %{_&}%2Adquira VIP e COINS acessando: %{_&}%fwww.kldev.tk         \n "
		set yml value "options.kit.none" of file "{@lang-file}" to "Nenhum "
		set yml value "options.kit.admin" of file "{@lang-file}" to "%{_&}%9Admin"
		set yml value "options.kit.fps" of file "{@lang-file}" to "FPS "
		set yml value "options.kit.lava-challenge" of file "{@lang-file}" to "Lava Challenge "
		set yml value "options.kit.knockback" of file "{@lang-file}" to "Knockback "
		set yml value "options.messages.select-kit" of file "{@lang-file}" to "%{_&}%a* O kit %{_&}%n{kit}%{_&}%a foi selecionado"
		set yml value "options.messages.enable-kit-skill" of file "{@lang-file}" to "%{_&}%e* Habilidade %{_&}%n{kit}%{_&}%e ativada"
		set yml value "options.messages.no-perm" of file "{@lang-file}" to "%{_&}%cVoce nao tem permissao para isto."
		set yml value "options.messages.warps-teleport.delay" of file "{@lang-file}" to "%{_&}%aAguarde %{_&}%c{time} %{_&}%asegundos."
		set yml value "options.messages.warps-teleport.sucess" of file "{@lang-file}" to "%{_&}%aTeleportado com sucesso!"
		set yml value "options.messages.fly.enabled" of file "{@lang-file}" to "%{_&}%aModo voar ativado com sucesso"
		set yml value "options.messages.fly.disabled" of file "{@lang-file}" to "%{_&}%cModo voar desativado com sucesso"
		set yml value "options.messages.build.enabled" of file "{@lang-file}" to "%{_&}%aModo construtor ativado com sucesso"
		set yml value "options.messages.build.disabled" of file "{@lang-file}" to "%{_&}%cModo construtor desativado com sucesso"
		set yml value "options.signs.recraft.line-1" of file "{@lang-file}" to "%{_&}%b=-=-=-=-="
		set yml value "options.signs.recraft.line-2" of file "{@lang-file}" to "%{_&}%aPlaca"
		set yml value "options.signs.recraft.line-3" of file "{@lang-file}" to "%{_&}%9(recraft)"
		set yml value "options.signs.recraft.line-4" of file "{@lang-file}" to "%{_&}%b=-=-=-=-="
		set yml value "options.signs.recraft.title" of file "{@lang-file}" to "Recraft "
		set yml value "options.signs.recraft.items.red-mushroom" of file "{@lang-file}" to "%{_&}%eCogumelo Vermelho"
		set yml value "options.signs.recraft.items.brown-mushroom" of file "{@lang-file}" to "%{_&}%eCogumelo Marrom"
		set yml value "options.signs.recraft.items.bowl" of file "{@lang-file}" to "%{_&}%ePote"
		set yml value "options.signs.refill.line-1" of file "{@lang-file}" to "%{_&}%b=-=-=-=-="
		set yml value "options.signs.refill.line-2" of file "{@lang-file}" to "%{_&}%aPlaca"
		set yml value "options.signs.refill.line-3" of file "{@lang-file}" to "%{_&}%9(refill)"
		set yml value "options.signs.refill.line-4" of file "{@lang-file}" to "%{_&}%b=-=-=-=-="
		set yml value "options.signs.refill.title" of file "{@lang-file}" to "Refill "
		set yml value "options.signs.refill.items.soup" of file "{@lang-file}" to "%{_&}%eSopa"
	if {_do} is "config-create":
		set yml value "check-update" of file "{@config-file}" to true
		set yml value "version" of file "{@config-file}" to "{@versao}"
		skutilities write "" at line 1 to file "{@config-file}"
		skutilities write "## Skript feito por KickHolse" at line 2 to file "{@config-file}"
		skutilities write "## https://discord.club/i/kldev" at line 3 to file "{@config-file}"
		skutilities write "" at line 4 to file "{@config-file}"
	if {_do} is "groups-create":
		set yaml value "groups.default.color" from file "{@groups-file}" to "%{_&}%7"
		set yaml value "groups.default.name" from file "{@groups-file}" to "Default"
		set yaml value "groups.default.prefix" from file "{@groups-file}" to "%{_&}%7"
		set yaml value "groups.default.permission" from file "{@groups-file}" to "none"
		set yaml value "groups.vip.color" from file "{@groups-file}" to "%{_&}%6"
		set yaml value "groups.vip.name" from file "{@groups-file}" to "Vip"
		set yaml value "groups.vip.prefix" from file "{@groups-file}" to "%{_&}%6[Vip] "
		set yaml value "groups.vip.permission" from file "{@groups-file}" to "group.vip"
		set yaml value "groups.dono.color" from file "{@groups-file}" to "%{_&}%4"
		set yaml value "groups.dono.name" from file "{@groups-file}" to "Dono"
		set yaml value "groups.dono.prefix" from file "{@groups-file}" to "%{_&}%4&lDONO&4 "
		set yaml value "groups.dono.permission" from file "{@groups-file}" to "*"
#━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━#
# inicialização
on skript load:
	getPrice()
on load:
	loop all players:
		showScoreboard(loop-player)
	if file "{@lang-file}" doesn't exist:
		create file "{@lang-file}"
		khkits_setvalue("lang-create")
	if file "{@groups-file}" doesn't exist:
		create file "{@groups-file}"
		khkits_setvalue("groups-create")
	if file "{@config-file}" doesn't exist:
		create file "{@config-file}"
		khkits_setvalue("config-create")
	khkits_setvalue("setvalue")
	set {khkp::version} to yml value "version" of file "{@config-file}"
	if {khkp::version} is not "{@versao}":
		set yml value "version" of file "{@config-file}" to "{@versao}"
	send "&3[Kh:KITS] &eVerificando atualizacoes..." to console
	wait 2 seconds
	if text from "https://api.spigotmc.org/legacy/update.php?resource=67595" is not "{@versao}":
		send "&3[Kh:KITS] &cVoce nao esta usando a ultima versao do skript." to console
	else:
		send "&3[Kh:KITS] &aVoce esta usando a ultima versao do skript." to console
	wait 2 seconds
	send "&3[Kh:KITS] &aKhKits &8{@versao} &aativado!" to console
on join:
	if text from "https://api.spigotmc.org/legacy/update.php?resource=67595" is not "{@versao}":
		set {khkp::updater} to true
		if {khkp::checkversion} is true:
			if player has permission "khkits.update" or "khkits.*":
				send "&6[KhKits] &7Existe &7uma &7nova &7atualização &7do &7skript, &7para &7visitar &7o &7site &7clique <tooltip:&bClique aqui para ir ao site de download.><url:https://www.spigotmc.org/resources/khkits-skript-de-kitpvp.67595/>&a&lAQUI!<reset>" to player
function intSpliter(n: text) :: text:
	set {_text} to "%{_n}%"
	set {_string::*} to {_text} split at ""
	set {_amount} to amount of {_string::*}
	remove 1 from {_amount}
	if {_amount} = 1:
		set {_spliting} to "%{_string::1}%"
	if {_amount} = 2:
		set {_spliting} to "%{_string::1}%%{_string::2}%"
	if {_amount} = 3:
		set {_spliting} to "%{_string::1}%%{_string::2}%%{_string::3}%"
	if {_amount} = 4:
		set {_spliting} to "%{_string::1}%,%{_string::2}%%{_string::3}%%{_string::4}%"
	if {_amount} = 5:
		set {_spliting} to "%{_string::1}%%{_string::2}%,%{_string::3}%%{_string::4}%%{_string::5}%" parsed as text
	if {_amount} = 6:
		set {_spliting} to "%{_string::1}%%{_string::2}%%{_string::3}%,%{_string::4}%%{_string::5}%%{_string::6}%" parsed as text
	if {_amount} = 7:
		set {_spliting} to "%{_string::1}%,%{_string::2}%%{_string::3}%%{_string::4}%,%{_string::5}%%{_string::6}%%{_string::7}%" parsed as text
	if {_amount} = 8:
		set {_spliting} to "%{_string::1}%%{_string::2}%,%{_string::3}%%{_string::4}%%{_string::5}%,%{_string::6}%%{_string::7}%%{_string::8}%" parsed as text
	if {_amount} = 9:
		set {_spliting} to "%{_string::1}%%{_string::2}%%{_string::3}%,%{_string::4}%%{_string::5}%%{_string::6}%,%{_string::7}%%{_string::8}%%{_string::9}%" parsed as text
	if {_amount} = 10:
		set {_spliting} to "%{_string::1}%,%{_string::2}%%{_string::3}%%{_string::4}%,%{_string::5}%%{_string::6}%%{_string::7}%,%{_string::8}%%{_string::9}%%{_string::10}%" parsed as text
	if {_amount} = 11:
		set {_spliting} to "%{_string::1}%%{_string::2}%,%{_string::3}%%{_string::4}%%{_string::5}%,%{_string::6}%%{_string::7}%%{_string::8}%,%{_string::9}%%{_string::10}%%{_string::11}%" parsed as text
	if {_amount} = 12:
		set {_spliting} to "%{_string::1}%%{_string::2}%%{_string::3}%,%{_string::4}%%{_string::5}%%{_string::6}%,%{_string::7}%%{_string::8}%%{_string::9}%,%{_string::10}%%{_string::11}%%{_string::12}%" parsed as text
	return "%{_spliting}%"
