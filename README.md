Si tu regardes le code, il n y a que 4 steps source, detection, pipeline et save
1.ok
2. actuellement si tu regardes le code, le flow final de source
	selection type source file, source name
	a. source (selection type source, upload file ou sql)
	b. detection&parametre ou on a le tableau avec selection des colonnes,display name,column,type,format, distinct ,nin max %null
	c. pipeline avec tableau pour afficher resulat des transformation
	d. save
3. OPTION A actuellement si tu regardes le code, le flow final de source sql
		selection type source  sql - source name
	
	a. source (selection type source, sql, connection, mode (sql query,table selection -si table selection, il affiche schema et tables a selectionnrt )
	b. detection&parametre ou on a le tableau avec selection des colonnes,display name,column,type,format, distinct ,nin max %null IMPORTANT SQL REQUET PEUT AVOIR DES PARAMETRES, a prendrre en compte par la suite
	c. pipeline avec tableau pour afficher resulat des transformation
	d. save
 
il faudra prendre en consideration qu on pourrait avoir dautre type de source comme api. donc lors du codage, faire en sorte que ce soit scallable et generique
4. ok meme flow que source et sql  source, detection, pipeline et save (pas de step colonne, deja inclus dans detection, )
5. source, detection,merge setup (preview tableau avec bouton refresh quand on change le merge setup), pipeline et save (pas de step colonne, deja inclus dans detection)
6. source, reconciliation setup (selection des clés, rules compare)preview tableau avec bouton refresh quand on change le reconciliation setup), pipeline et save (pas de step colonne, deja inclus dans detection)
7. A navigation libre
8.ok
9. je te laisse le choix de prendre la meilleure decision
10. ok
11. ok mais la parametre run, ce sera pas dans le setup mais dans sous menu view ou on rajoutera un bouton run
12. ok 
13. ok mais pas de save draft et les boutons next et back doivent etre situé au meme endroit quelque soit le step
14. c deja bien dans le code, rien a modifier
