---
title: "dplyr Superhero"
date: "2023-01-24"
output:
  html_document: 
    theme: spacelab
    toc: yes
    toc_float: yes
    keep_md: yes
---

## Learning Goals  
*At the end of this exercise, you will be able to:*    
1. Develop your dplyr superpowers so you can easily and confidently manipulate dataframes.  
2. Learn helpful new functions that are part of the `janitor` package.  

## Instructions
For the second part of lab 5 today, we are going to spend time practicing the dplyr functions we have learned and add a few new ones. We will spend most of the time in our breakout rooms. Your lab 5 homework will be to knit and push this file to your repository.  

## Load the tidyverse

```r
library("tidyverse")
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
## ✔ ggplot2 3.4.0      ✔ purrr   1.0.1 
## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
## ✔ tidyr   1.2.1      ✔ stringr 1.5.0 
## ✔ readr   2.1.3      ✔ forcats 0.5.2 
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

## Load the superhero data
These are data taken from comic books and assembled by fans. The include a good mix of categorical and continuous data.  Data taken from: https://www.kaggle.com/claudiodavi/superhero-set  

Check out the way I am loading these data. If I know there are NAs, I can take care of them at the beginning. But, we should do this very cautiously. At times it is better to keep the original columns and data intact.  

```r
superhero_info <- readr::read_csv("data/heroes_information.csv", na = c("", "-99", "-"))
```

```
## Rows: 734 Columns: 10
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (8): name, Gender, Eye color, Race, Hair color, Publisher, Skin color, A...
## dbl (2): Height, Weight
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
superhero_powers <- readr::read_csv("data/super_hero_powers.csv", na = c("", "-99", "-"))
```

```
## Rows: 667 Columns: 168
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr   (1): hero_names
## lgl (167): Agility, Accelerated Healing, Lantern Power Ring, Dimensional Awa...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

## Data tidy
1. Some of the names used in the `superhero_info` data are problematic so you should rename them here.  

```r
names(superhero_info)
```

```
##  [1] "name"       "Gender"     "Eye color"  "Race"       "Hair color"
##  [6] "Height"     "Publisher"  "Skin color" "Alignment"  "Weight"
```

```r
superhero_info<-rename(superhero_info,name="name",gender="Gender",eye_color="Eye color",race="Race",hair_color="Hair color",height="Height",publisher="Publisher",skin_color="Skin color",alignment="Alignment",weight="Weight")
#superhero_info<-janitor::clean_names(superhero_info)
superhero_info
```

```
## # A tibble: 734 × 10
##    name       gender eye_c…¹ race  hair_…² height publi…³ skin_…⁴ align…⁵ weight
##    <chr>      <chr>  <chr>   <chr> <chr>    <dbl> <chr>   <chr>   <chr>    <dbl>
##  1 A-Bomb     Male   yellow  Human No Hair    203 Marvel… <NA>    good       441
##  2 Abe Sapien Male   blue    Icth… No Hair    191 Dark H… blue    good        65
##  3 Abin Sur   Male   blue    Unga… No Hair    185 DC Com… red     good        90
##  4 Abominati… Male   green   Huma… No Hair    203 Marvel… <NA>    bad        441
##  5 Abraxas    Male   blue    Cosm… Black       NA Marvel… <NA>    bad         NA
##  6 Absorbing… Male   blue    Human No Hair    193 Marvel… <NA>    bad        122
##  7 Adam Monr… Male   blue    <NA>  Blond       NA NBC - … <NA>    good        NA
##  8 Adam Stra… Male   blue    Human Blond      185 DC Com… <NA>    good        88
##  9 Agent 13   Female blue    <NA>  Blond      173 Marvel… <NA>    good        61
## 10 Agent Bob  Male   brown   Human Brown      178 Marvel… <NA>    good        81
## # … with 724 more rows, and abbreviated variable names ¹​eye_color, ²​hair_color,
## #   ³​publisher, ⁴​skin_color, ⁵​alignment
```

Yikes! `superhero_powers` has a lot of variables that are poorly named. We need some R superpowers...

```r
head(superhero_powers)
```

```
## # A tibble: 6 × 168
##   hero_…¹ Agility Accel…² Lante…³ Dimen…⁴ Cold …⁵ Durab…⁶ Stealth Energ…⁷ Flight
##   <chr>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl> 
## 1 3-D Man TRUE    FALSE   FALSE   FALSE   FALSE   FALSE   FALSE   FALSE   FALSE 
## 2 A-Bomb  FALSE   TRUE    FALSE   FALSE   FALSE   TRUE    FALSE   FALSE   FALSE 
## 3 Abe Sa… TRUE    TRUE    FALSE   FALSE   TRUE    TRUE    FALSE   FALSE   FALSE 
## 4 Abin S… FALSE   FALSE   TRUE    FALSE   FALSE   FALSE   FALSE   FALSE   FALSE 
## 5 Abomin… FALSE   TRUE    FALSE   FALSE   FALSE   FALSE   FALSE   FALSE   FALSE 
## 6 Abraxas FALSE   FALSE   FALSE   TRUE    FALSE   FALSE   FALSE   FALSE   TRUE  
## # … with 158 more variables: `Danger Sense` <lgl>,
## #   `Underwater breathing` <lgl>, Marksmanship <lgl>, `Weapons Master` <lgl>,
## #   `Power Augmentation` <lgl>, `Animal Attributes` <lgl>, Longevity <lgl>,
## #   Intelligence <lgl>, `Super Strength` <lgl>, Cryokinesis <lgl>,
## #   Telepathy <lgl>, `Energy Armor` <lgl>, `Energy Blasts` <lgl>,
## #   Duplication <lgl>, `Size Changing` <lgl>, `Density Control` <lgl>,
## #   Stamina <lgl>, `Astral Travel` <lgl>, `Audio Control` <lgl>, …
```

## `janitor`
The [janitor](https://garthtarr.github.io/meatR/janitor.html) package is your friend. Make sure to install it and then load the library.  



```r
library("janitor")
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

The `clean_names` function takes care of everything in one line! Now that's a superpower!

```r
superhero_powers <- janitor::clean_names(superhero_powers)
names(superhero_powers)
```

```
##   [1] "hero_names"                   "agility"                     
##   [3] "accelerated_healing"          "lantern_power_ring"          
##   [5] "dimensional_awareness"        "cold_resistance"             
##   [7] "durability"                   "stealth"                     
##   [9] "energy_absorption"            "flight"                      
##  [11] "danger_sense"                 "underwater_breathing"        
##  [13] "marksmanship"                 "weapons_master"              
##  [15] "power_augmentation"           "animal_attributes"           
##  [17] "longevity"                    "intelligence"                
##  [19] "super_strength"               "cryokinesis"                 
##  [21] "telepathy"                    "energy_armor"                
##  [23] "energy_blasts"                "duplication"                 
##  [25] "size_changing"                "density_control"             
##  [27] "stamina"                      "astral_travel"               
##  [29] "audio_control"                "dexterity"                   
##  [31] "omnitrix"                     "super_speed"                 
##  [33] "possession"                   "animal_oriented_powers"      
##  [35] "weapon_based_powers"          "electrokinesis"              
##  [37] "darkforce_manipulation"       "death_touch"                 
##  [39] "teleportation"                "enhanced_senses"             
##  [41] "telekinesis"                  "energy_beams"                
##  [43] "magic"                        "hyperkinesis"                
##  [45] "jump"                         "clairvoyance"                
##  [47] "dimensional_travel"           "power_sense"                 
##  [49] "shapeshifting"                "peak_human_condition"        
##  [51] "immortality"                  "camouflage"                  
##  [53] "element_control"              "phasing"                     
##  [55] "astral_projection"            "electrical_transport"        
##  [57] "fire_control"                 "projection"                  
##  [59] "summoning"                    "enhanced_memory"             
##  [61] "reflexes"                     "invulnerability"             
##  [63] "energy_constructs"            "force_fields"                
##  [65] "self_sustenance"              "anti_gravity"                
##  [67] "empathy"                      "power_nullifier"             
##  [69] "radiation_control"            "psionic_powers"              
##  [71] "elasticity"                   "substance_secretion"         
##  [73] "elemental_transmogrification" "technopath_cyberpath"        
##  [75] "photographic_reflexes"        "seismic_power"               
##  [77] "animation"                    "precognition"                
##  [79] "mind_control"                 "fire_resistance"             
##  [81] "power_absorption"             "enhanced_hearing"            
##  [83] "nova_force"                   "insanity"                    
##  [85] "hypnokinesis"                 "animal_control"              
##  [87] "natural_armor"                "intangibility"               
##  [89] "enhanced_sight"               "molecular_manipulation"      
##  [91] "heat_generation"              "adaptation"                  
##  [93] "gliding"                      "power_suit"                  
##  [95] "mind_blast"                   "probability_manipulation"    
##  [97] "gravity_control"              "regeneration"                
##  [99] "light_control"                "echolocation"                
## [101] "levitation"                   "toxin_and_disease_control"   
## [103] "banish"                       "energy_manipulation"         
## [105] "heat_resistance"              "natural_weapons"             
## [107] "time_travel"                  "enhanced_smell"              
## [109] "illusions"                    "thirstokinesis"              
## [111] "hair_manipulation"            "illumination"                
## [113] "omnipotent"                   "cloaking"                    
## [115] "changing_armor"               "power_cosmic"                
## [117] "biokinesis"                   "water_control"               
## [119] "radiation_immunity"           "vision_telescopic"           
## [121] "toxin_and_disease_resistance" "spatial_awareness"           
## [123] "energy_resistance"            "telepathy_resistance"        
## [125] "molecular_combustion"         "omnilingualism"              
## [127] "portal_creation"              "magnetism"                   
## [129] "mind_control_resistance"      "plant_control"               
## [131] "sonar"                        "sonic_scream"                
## [133] "time_manipulation"            "enhanced_touch"              
## [135] "magic_resistance"             "invisibility"                
## [137] "sub_mariner"                  "radiation_absorption"        
## [139] "intuitive_aptitude"           "vision_microscopic"          
## [141] "melting"                      "wind_control"                
## [143] "super_breath"                 "wallcrawling"                
## [145] "vision_night"                 "vision_infrared"             
## [147] "grim_reaping"                 "matter_absorption"           
## [149] "the_force"                    "resurrection"                
## [151] "terrakinesis"                 "vision_heat"                 
## [153] "vitakinesis"                  "radar_sense"                 
## [155] "qwardian_power_ring"          "weather_control"             
## [157] "vision_x_ray"                 "vision_thermal"              
## [159] "web_creation"                 "reality_warping"             
## [161] "odin_force"                   "symbiote_costume"            
## [163] "speed_force"                  "phoenix_force"               
## [165] "molecular_dissipation"        "vision_cryo"                 
## [167] "omnipresent"                  "omniscient"
```

## `tabyl`
The `janitor` package has many awesome functions that we will explore. Here is its version of `table` which not only produces counts but also percentages. Very handy! Let's use it to explore the proportion of good guys and bad guys in the `superhero_info` data.  


```r
tabyl(superhero_info, alignment)
```

```
##  alignment   n     percent valid_percent
##        bad 207 0.282016349    0.28473177
##       good 496 0.675749319    0.68225585
##    neutral  24 0.032697548    0.03301238
##       <NA>   7 0.009536785            NA
```

2. Notice that we have some neutral superheros! Who are they?

```r
neutral_superhero<- superhero_info %>% 
  filter(alignment=="neutral")
neutral_superhero
```

```
## # A tibble: 24 × 10
##    name       gender eye_c…¹ race  hair_…² height publi…³ skin_…⁴ align…⁵ weight
##    <chr>      <chr>  <chr>   <chr> <chr>    <dbl> <chr>   <chr>   <chr>    <dbl>
##  1 Bizarro    Male   black   Biza… Black      191 DC Com… white   neutral    155
##  2 Black Fla… Male   <NA>    God … <NA>        NA DC Com… <NA>    neutral     NA
##  3 Captain C… Male   brown   Human Brown       NA DC Com… <NA>    neutral     NA
##  4 Copycat    Female red     Muta… White      183 Marvel… blue    neutral     67
##  5 Deadpool   Male   brown   Muta… No Hair    188 Marvel… <NA>    neutral     95
##  6 Deathstro… Male   blue    Human White      193 DC Com… <NA>    neutral    101
##  7 Etrigan    Male   red     Demon No Hair    193 DC Com… yellow  neutral    203
##  8 Galactus   Male   black   Cosm… Black      876 Marvel… <NA>    neutral     16
##  9 Gladiator  Male   blue    Stro… Blue       198 Marvel… purple  neutral    268
## 10 Indigo     Female <NA>    Alien Purple      NA DC Com… <NA>    neutral     NA
## # … with 14 more rows, and abbreviated variable names ¹​eye_color, ²​hair_color,
## #   ³​publisher, ⁴​skin_color, ⁵​alignment
```

## `superhero_info`
3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?

```r
superhero_info_2<-superhero_info %>% 
  select(name,alignment,race)
superhero_info_2
```

```
## # A tibble: 734 × 3
##    name          alignment race             
##    <chr>         <chr>     <chr>            
##  1 A-Bomb        good      Human            
##  2 Abe Sapien    good      Icthyo Sapien    
##  3 Abin Sur      good      Ungaran          
##  4 Abomination   bad       Human / Radiation
##  5 Abraxas       bad       Cosmic Entity    
##  6 Absorbing Man bad       Human            
##  7 Adam Monroe   good      <NA>             
##  8 Adam Strange  good      Human            
##  9 Agent 13      good      <NA>             
## 10 Agent Bob     good      Human            
## # … with 724 more rows
```

## Not Human
4. List all of the superheros that are not human.

```r
superhero_info_2 %>% 
  filter(race != "Human")
```

```
## # A tibble: 222 × 3
##    name         alignment race             
##    <chr>        <chr>     <chr>            
##  1 Abe Sapien   good      Icthyo Sapien    
##  2 Abin Sur     good      Ungaran          
##  3 Abomination  bad       Human / Radiation
##  4 Abraxas      bad       Cosmic Entity    
##  5 Ajax         bad       Cyborg           
##  6 Alien        bad       Xenomorph XX121  
##  7 Amazo        bad       Android          
##  8 Angel        good      Vampire          
##  9 Angel Dust   good      Mutant           
## 10 Anti-Monitor bad       God / Eternal    
## # … with 212 more rows
```

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".

```r
good_guys<-superhero_info %>% 
  filter(alignment == "good")
good_guys
```

```
## # A tibble: 496 × 10
##    name       gender eye_c…¹ race  hair_…² height publi…³ skin_…⁴ align…⁵ weight
##    <chr>      <chr>  <chr>   <chr> <chr>    <dbl> <chr>   <chr>   <chr>    <dbl>
##  1 A-Bomb     Male   yellow  Human No Hair    203 Marvel… <NA>    good       441
##  2 Abe Sapien Male   blue    Icth… No Hair    191 Dark H… blue    good        65
##  3 Abin Sur   Male   blue    Unga… No Hair    185 DC Com… red     good        90
##  4 Adam Monr… Male   blue    <NA>  Blond       NA NBC - … <NA>    good        NA
##  5 Adam Stra… Male   blue    Human Blond      185 DC Com… <NA>    good        88
##  6 Agent 13   Female blue    <NA>  Blond      173 Marvel… <NA>    good        61
##  7 Agent Bob  Male   brown   Human Brown      178 Marvel… <NA>    good        81
##  8 Agent Zero Male   <NA>    <NA>  <NA>       191 Marvel… <NA>    good       104
##  9 Alan Scott Male   blue    <NA>  Blond      180 DC Com… <NA>    good        90
## 10 Alex Wool… Male   <NA>    <NA>  <NA>        NA NBC - … <NA>    good        NA
## # … with 486 more rows, and abbreviated variable names ¹​eye_color, ²​hair_color,
## #   ³​publisher, ⁴​skin_color, ⁵​alignment
```


```r
bad_guys<-superhero_info %>% 
  filter(alignment == "bad")
bad_guys
```

```
## # A tibble: 207 × 10
##    name       gender eye_c…¹ race  hair_…² height publi…³ skin_…⁴ align…⁵ weight
##    <chr>      <chr>  <chr>   <chr> <chr>    <dbl> <chr>   <chr>   <chr>    <dbl>
##  1 Abominati… Male   green   Huma… No Hair    203 Marvel… <NA>    bad        441
##  2 Abraxas    Male   blue    Cosm… Black       NA Marvel… <NA>    bad         NA
##  3 Absorbing… Male   blue    Human No Hair    193 Marvel… <NA>    bad        122
##  4 Air-Walker Male   blue    <NA>  White      188 Marvel… <NA>    bad        108
##  5 Ajax       Male   brown   Cybo… Black      193 Marvel… <NA>    bad         90
##  6 Alex Merc… Male   <NA>    Human <NA>        NA Wildst… <NA>    bad         NA
##  7 Alien      Male   <NA>    Xeno… No Hair    244 Dark H… black   bad        169
##  8 Amazo      Male   red     Andr… <NA>       257 DC Com… <NA>    bad        173
##  9 Ammo       Male   brown   Human Black      188 Marvel… <NA>    bad        101
## 10 Angela     Female <NA>    <NA>  <NA>        NA Image … <NA>    bad         NA
## # … with 197 more rows, and abbreviated variable names ¹​eye_color, ²​hair_color,
## #   ³​publisher, ⁴​skin_color, ⁵​alignment
```

6. For the good guys, use the `tabyl` function to summarize their "race".

```r
tabyl(good_guys, "race")
```

```
##               race   n     percent valid_percent
##              Alien   3 0.006048387   0.010752688
##              Alpha   5 0.010080645   0.017921147
##             Amazon   2 0.004032258   0.007168459
##            Android   4 0.008064516   0.014336918
##             Animal   2 0.004032258   0.007168459
##          Asgardian   3 0.006048387   0.010752688
##          Atlantean   4 0.008064516   0.014336918
##         Bolovaxian   1 0.002016129   0.003584229
##              Clone   1 0.002016129   0.003584229
##             Cyborg   3 0.006048387   0.010752688
##           Demi-God   2 0.004032258   0.007168459
##              Demon   3 0.006048387   0.010752688
##            Eternal   1 0.002016129   0.003584229
##     Flora Colossus   1 0.002016129   0.003584229
##        Frost Giant   1 0.002016129   0.003584229
##      God / Eternal   6 0.012096774   0.021505376
##             Gungan   1 0.002016129   0.003584229
##              Human 148 0.298387097   0.530465950
##    Human / Altered   2 0.004032258   0.007168459
##     Human / Cosmic   2 0.004032258   0.007168459
##  Human / Radiation   8 0.016129032   0.028673835
##         Human-Kree   2 0.004032258   0.007168459
##      Human-Spartoi   1 0.002016129   0.003584229
##       Human-Vulcan   1 0.002016129   0.003584229
##    Human-Vuldarian   1 0.002016129   0.003584229
##      Icthyo Sapien   1 0.002016129   0.003584229
##            Inhuman   4 0.008064516   0.014336918
##    Kakarantharaian   1 0.002016129   0.003584229
##         Kryptonian   4 0.008064516   0.014336918
##            Martian   1 0.002016129   0.003584229
##          Metahuman   1 0.002016129   0.003584229
##             Mutant  46 0.092741935   0.164874552
##     Mutant / Clone   1 0.002016129   0.003584229
##             Planet   1 0.002016129   0.003584229
##             Saiyan   1 0.002016129   0.003584229
##           Symbiote   3 0.006048387   0.010752688
##           Talokite   1 0.002016129   0.003584229
##         Tamaranean   1 0.002016129   0.003584229
##            Ungaran   1 0.002016129   0.003584229
##            Vampire   2 0.004032258   0.007168459
##     Yoda's species   1 0.002016129   0.003584229
##      Zen-Whoberian   1 0.002016129   0.003584229
##               <NA> 217 0.437500000            NA
```

7. Among the good guys, Who are the Asgardians?

```r
good_guys %>% 
  filter(race =="Asgardian")
```

```
## # A tibble: 3 × 10
##   name      gender eye_color race  hair_…¹ height publi…² skin_…³ align…⁴ weight
##   <chr>     <chr>  <chr>     <chr> <chr>    <dbl> <chr>   <chr>   <chr>    <dbl>
## 1 Sif       Female blue      Asga… Black      188 Marvel… <NA>    good       191
## 2 Thor      Male   blue      Asga… Blond      198 Marvel… <NA>    good       288
## 3 Thor Girl Female blue      Asga… Blond      175 Marvel… <NA>    good       143
## # … with abbreviated variable names ¹​hair_color, ²​publisher, ³​skin_color,
## #   ⁴​alignment
```

8. Among the bad guys, who are the male humans over 200 inches in height?

```r
glimpse(bad_guys)
```

```
## Rows: 207
## Columns: 10
## $ name       <chr> "Abomination", "Abraxas", "Absorbing Man", "Air-Walker", "A…
## $ gender     <chr> "Male", "Male", "Male", "Male", "Male", "Male", "Male", "Ma…
## $ eye_color  <chr> "green", "blue", "blue", "blue", "brown", NA, NA, "red", "b…
## $ race       <chr> "Human / Radiation", "Cosmic Entity", "Human", NA, "Cyborg"…
## $ hair_color <chr> "No Hair", "Black", "No Hair", "White", "Black", NA, "No Ha…
## $ height     <dbl> 203, NA, 193, 188, 193, NA, 244, 257, 188, NA, 180, 61, NA,…
## $ publisher  <chr> "Marvel Comics", "Marvel Comics", "Marvel Comics", "Marvel …
## $ skin_color <chr> NA, NA, NA, NA, NA, NA, "black", NA, NA, NA, NA, NA, NA, "g…
## $ alignment  <chr> "bad", "bad", "bad", "bad", "bad", "bad", "bad", "bad", "ba…
## $ weight     <dbl> 441, NA, 122, 108, 90, NA, 169, 173, 101, NA, 90, NA, NA, 1…
```

```r
bad_guys %>% 
  filter(gender=="Male"&race == "Human") %>% 
  filter(height>200)
```

```
## # A tibble: 5 × 10
##   name        gender eye_c…¹ race  hair_…² height publi…³ skin_…⁴ align…⁵ weight
##   <chr>       <chr>  <chr>   <chr> <chr>    <dbl> <chr>   <chr>   <chr>    <dbl>
## 1 Bane        Male   <NA>    Human <NA>       203 DC Com… <NA>    bad        180
## 2 Doctor Doom Male   brown   Human Brown      201 Marvel… <NA>    bad        187
## 3 Kingpin     Male   blue    Human No Hair    201 Marvel… <NA>    bad        203
## 4 Lizard      Male   red     Human No Hair    203 Marvel… <NA>    bad        230
## 5 Scorpion    Male   brown   Human Brown      211 Marvel… <NA>    bad        310
## # … with abbreviated variable names ¹​eye_color, ²​hair_color, ³​publisher,
## #   ⁴​skin_color, ⁵​alignment
```


9. OK, so are there more good guys or bad guys that are bald (personal interest)?
There are more good guys that are bald than bad guys.

```r
good_guys %>% 
  filter(hair_color=="No Hair") %>% 
  tabyl(hair_color)
```

```
##  hair_color  n percent
##     No Hair 37       1
```


```r
bad_guys %>% 
  filter(hair_color=="No Hair") %>% 
  tabyl(hair_color)
```

```
##  hair_color  n percent
##     No Hair 35       1
```


10. Let's explore who the really "big" superheros are. In the `superhero_info` data, which have a height over 200 or weight greater than or equal to 450?

```r
glimpse(superhero_info)
```

```
## Rows: 734
## Columns: 10
## $ name       <chr> "A-Bomb", "Abe Sapien", "Abin Sur", "Abomination", "Abraxas…
## $ gender     <chr> "Male", "Male", "Male", "Male", "Male", "Male", "Male", "Ma…
## $ eye_color  <chr> "yellow", "blue", "blue", "green", "blue", "blue", "blue", …
## $ race       <chr> "Human", "Icthyo Sapien", "Ungaran", "Human / Radiation", "…
## $ hair_color <chr> "No Hair", "No Hair", "No Hair", "No Hair", "Black", "No Ha…
## $ height     <dbl> 203, 191, 185, 203, NA, 193, NA, 185, 173, 178, 191, 188, 1…
## $ publisher  <chr> "Marvel Comics", "Dark Horse Comics", "DC Comics", "Marvel …
## $ skin_color <chr> NA, "blue", "red", NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ alignment  <chr> "good", "good", "good", "bad", "bad", "bad", "good", "good"…
## $ weight     <dbl> 441, 65, 90, 441, NA, 122, NA, 88, 61, 81, 104, 108, 90, 90…
```


```r
big_superheroes<-superhero_info %>% 
  filter(height>200|weight>=450)
big_superheroes
```

```
## # A tibble: 60 × 10
##    name       gender eye_c…¹ race  hair_…² height publi…³ skin_…⁴ align…⁵ weight
##    <chr>      <chr>  <chr>   <chr> <chr>    <dbl> <chr>   <chr>   <chr>    <dbl>
##  1 A-Bomb     Male   yellow  Human No Hair    203 Marvel… <NA>    good       441
##  2 Abominati… Male   green   Huma… No Hair    203 Marvel… <NA>    bad        441
##  3 Alien      Male   <NA>    Xeno… No Hair    244 Dark H… black   bad        169
##  4 Amazo      Male   red     Andr… <NA>       257 DC Com… <NA>    bad        173
##  5 Ant-Man    Male   blue    Human Blond      211 Marvel… <NA>    good       122
##  6 Anti-Venom Male   blue    Symb… Blond      229 Marvel… <NA>    <NA>       358
##  7 Apocalypse Male   red     Muta… Black      213 Marvel… grey    bad        135
##  8 Bane       Male   <NA>    Human <NA>       203 DC Com… <NA>    bad        180
##  9 Beta Ray … Male   <NA>    <NA>  No Hair    201 Marvel… <NA>    good       216
## 10 Bloodaxe   Female blue    Human Brown      218 Marvel… <NA>    bad        495
## # … with 50 more rows, and abbreviated variable names ¹​eye_color, ²​hair_color,
## #   ³​publisher, ⁴​skin_color, ⁵​alignment
```

11. Just to be clear on the `|` operator,  have a look at the superheros over 300 in height...

```r
superhero_info %>% 
  filter(height>300)
```

```
## # A tibble: 8 × 10
##   name        gender eye_c…¹ race  hair_…² height publi…³ skin_…⁴ align…⁵ weight
##   <chr>       <chr>  <chr>   <chr> <chr>    <dbl> <chr>   <chr>   <chr>    <dbl>
## 1 Fin Fang F… Male   red     Kaka… No Hair   975  Marvel… green   good        18
## 2 Galactus    Male   black   Cosm… Black     876  Marvel… <NA>    neutral     16
## 3 Groot       Male   yellow  Flor… <NA>      701  Marvel… <NA>    good         4
## 4 MODOK       Male   white   Cybo… Brownn    366  Marvel… <NA>    bad        338
## 5 Onslaught   Male   red     Muta… No Hair   305  Marvel… <NA>    bad        405
## 6 Sasquatch   Male   red     <NA>  Orange    305  Marvel… <NA>    good       900
## 7 Wolfsbane   Female green   <NA>  Auburn    366  Marvel… <NA>    good       473
## 8 Ymir        Male   white   Fros… No Hair   305. Marvel… white   good        NA
## # … with abbreviated variable names ¹​eye_color, ²​hair_color, ³​publisher,
## #   ⁴​skin_color, ⁵​alignment
```

12. ...and the superheros over 450 in weight. Bonus question! Why do we not have 16 rows in question #10?
Because only two of the heroes fall in both categories.

```r
superhero_info %>% 
  filter(weight>450)
```

```
## # A tibble: 8 × 10
##   name       gender eye_co…¹ race  hair_…² height publi…³ skin_…⁴ align…⁵ weight
##   <chr>      <chr>  <chr>    <chr> <chr>    <dbl> <chr>   <chr>   <chr>    <dbl>
## 1 Bloodaxe   Female blue     Human Brown    218   Marvel… <NA>    bad        495
## 2 Darkseid   Male   red      New … No Hair  267   DC Com… grey    bad        817
## 3 Giganta    Female green    <NA>  Red       62.5 DC Com… <NA>    bad        630
## 4 Hulk       Male   green    Huma… Green    244   Marvel… green   good       630
## 5 Juggernaut Male   blue     Human Red      287   Marvel… <NA>    neutral    855
## 6 Red Hulk   Male   yellow   Huma… Black    213   Marvel… red     neutral    630
## 7 Sasquatch  Male   red      <NA>  Orange   305   Marvel… <NA>    good       900
## 8 Wolfsbane  Female green    <NA>  Auburn   366   Marvel… <NA>    good       473
## # … with abbreviated variable names ¹​eye_color, ²​hair_color, ³​publisher,
## #   ⁴​skin_color, ⁵​alignment
```

## Height to Weight Ratio
13. It's easy to be strong when you are heavy and tall, but who is heavy and short? Which superheros have the highest height to weight ratio?
Giganta has the most heavy and short.
Groot is the tallest and lightest

```r
superhero_info %>% 
  mutate(hw_ratio = height/weight) %>% 
  arrange(hw_ratio)
```

```
## # A tibble: 734 × 11
##    name       gender eye_c…¹ race  hair_…² height publi…³ skin_…⁴ align…⁵ weight
##    <chr>      <chr>  <chr>   <chr> <chr>    <dbl> <chr>   <chr>   <chr>    <dbl>
##  1 Giganta    Female green   <NA>  Red       62.5 DC Com… <NA>    bad        630
##  2 Utgard-Lo… Male   blue    Fros… White     15.2 Marvel… <NA>    bad         58
##  3 Darkseid   Male   red     New … No Hair  267   DC Com… grey    bad        817
##  4 Juggernaut Male   blue    Human Red      287   Marvel… <NA>    neutral    855
##  5 Red Hulk   Male   yellow  Huma… Black    213   Marvel… red     neutral    630
##  6 Sasquatch  Male   red     <NA>  Orange   305   Marvel… <NA>    good       900
##  7 Hulk       Male   green   Huma… Green    244   Marvel… green   good       630
##  8 Bloodaxe   Female blue    Human Brown    218   Marvel… <NA>    bad        495
##  9 Thanos     Male   red     Eter… No Hair  201   Marvel… purple  bad        443
## 10 A-Bomb     Male   yellow  Human No Hair  203   Marvel… <NA>    good       441
## # … with 724 more rows, 1 more variable: hw_ratio <dbl>, and abbreviated
## #   variable names ¹​eye_color, ²​hair_color, ³​publisher, ⁴​skin_color, ⁵​alignment
```

```r
superhero_info %>% 
  mutate(hw_ratio = height/weight) %>% 
  arrange(desc(hw_ratio))
```

```
## # A tibble: 734 × 11
##    name       gender eye_c…¹ race  hair_…² height publi…³ skin_…⁴ align…⁵ weight
##    <chr>      <chr>  <chr>   <chr> <chr>    <dbl> <chr>   <chr>   <chr>    <dbl>
##  1 Groot      Male   yellow  Flor… <NA>       701 Marvel… <NA>    good         4
##  2 Galactus   Male   black   Cosm… Black      876 Marvel… <NA>    neutral     16
##  3 Fin Fang … Male   red     Kaka… No Hair    975 Marvel… green   good        18
##  4 Longshot   Male   blue    Human Blond      188 Marvel… <NA>    good        36
##  5 Jack-Jack  Male   blue    Human Brown       71 Dark H… <NA>    good        14
##  6 Rocket Ra… Male   brown   Anim… Brown      122 Marvel… <NA>    good        25
##  7 Dash       Male   blue    Human Blond      122 Dark H… <NA>    good        27
##  8 Howard th… Male   brown   <NA>  Yellow      79 Marvel… <NA>    good        18
##  9 Swarm      Male   yellow  Muta… No Hair    196 Marvel… yellow  bad         47
## 10 Yoda       Male   brown   Yoda… White       66 George… green   good        17
## # … with 724 more rows, 1 more variable: hw_ratio <dbl>, and abbreviated
## #   variable names ¹​eye_color, ²​hair_color, ³​publisher, ⁴​skin_color, ⁵​alignment
```
## `superhero_powers`
Have a quick look at the `superhero_powers` data frame.  

```r
glimpse(superhero_powers)
```

```
## Rows: 667
## Columns: 168
## $ hero_names                   <chr> "3-D Man", "A-Bomb", "Abe Sapien", "Abin …
## $ agility                      <lgl> TRUE, FALSE, TRUE, FALSE, FALSE, FALSE, F…
## $ accelerated_healing          <lgl> FALSE, TRUE, TRUE, FALSE, TRUE, FALSE, FA…
## $ lantern_power_ring           <lgl> FALSE, FALSE, FALSE, TRUE, FALSE, FALSE, …
## $ dimensional_awareness        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ cold_resistance              <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ durability                   <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE, T…
## $ stealth                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ energy_absorption            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ flight                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ danger_sense                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ underwater_breathing         <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ marksmanship                 <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ weapons_master               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ power_augmentation           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ animal_attributes            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ longevity                    <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE, F…
## $ intelligence                 <lgl> FALSE, FALSE, TRUE, FALSE, TRUE, TRUE, FA…
## $ super_strength               <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, TRUE, TRUE…
## $ cryokinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ telepathy                    <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ energy_armor                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ energy_blasts                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ duplication                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ size_changing                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ density_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ stamina                      <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, FALSE, FAL…
## $ astral_travel                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ audio_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ dexterity                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ omnitrix                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ super_speed                  <lgl> TRUE, FALSE, FALSE, FALSE, TRUE, TRUE, FA…
## $ possession                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ animal_oriented_powers       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ weapon_based_powers          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ electrokinesis               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ darkforce_manipulation       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ death_touch                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ teleportation                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ enhanced_senses              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ telekinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ energy_beams                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ magic                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ hyperkinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ jump                         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ clairvoyance                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ dimensional_travel           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ power_sense                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ shapeshifting                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ peak_human_condition         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ immortality                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, TRUE, F…
## $ camouflage                   <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE, …
## $ element_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ phasing                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ astral_projection            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ electrical_transport         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ fire_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ projection                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ summoning                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ enhanced_memory              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ reflexes                     <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ invulnerability              <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, TRUE, T…
## $ energy_constructs            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ force_fields                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ self_sustenance              <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE, …
## $ anti_gravity                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ empathy                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ power_nullifier              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ radiation_control            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ psionic_powers               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ elasticity                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ substance_secretion          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ elemental_transmogrification <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ technopath_cyberpath         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ photographic_reflexes        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ seismic_power                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ animation                    <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALSE, …
## $ precognition                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ mind_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ fire_resistance              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ power_absorption             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ enhanced_hearing             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ nova_force                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ insanity                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ hypnokinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ animal_control               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ natural_armor                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ intangibility                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ enhanced_sight               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ molecular_manipulation       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ heat_generation              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ adaptation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ gliding                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ power_suit                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ mind_blast                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ probability_manipulation     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ gravity_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ regeneration                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ light_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ echolocation                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ levitation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ toxin_and_disease_control    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ banish                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ energy_manipulation          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ heat_resistance              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ natural_weapons              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ time_travel                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ enhanced_smell               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ illusions                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ thirstokinesis               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ hair_manipulation            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ illumination                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ omnipotent                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ cloaking                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ changing_armor               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ power_cosmic                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ biokinesis                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ water_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ radiation_immunity           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_telescopic            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ toxin_and_disease_resistance <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ spatial_awareness            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ energy_resistance            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ telepathy_resistance         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ molecular_combustion         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ omnilingualism               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ portal_creation              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ magnetism                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ mind_control_resistance      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ plant_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ sonar                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ sonic_scream                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ time_manipulation            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ enhanced_touch               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ magic_resistance             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ invisibility                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ sub_mariner                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ radiation_absorption         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ intuitive_aptitude           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_microscopic           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ melting                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ wind_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ super_breath                 <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALSE, …
## $ wallcrawling                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_night                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_infrared              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ grim_reaping                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ matter_absorption            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ the_force                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ resurrection                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ terrakinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_heat                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vitakinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ radar_sense                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ qwardian_power_ring          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ weather_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_x_ray                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_thermal               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ web_creation                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ reality_warping              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ odin_force                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ symbiote_costume             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ speed_force                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ phoenix_force                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ molecular_dissipation        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_cryo                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ omnipresent                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ omniscient                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
```

14. How many superheros have a combination of accelerated healing, durability, and super strength?

```r
superhero_powers %>% 
  filter(accelerated_healing==T&durability==T&super_strength==T)
```

```
## # A tibble: 97 × 168
##    hero_names   agility accele…¹ lante…² dimen…³ cold_…⁴ durab…⁵ stealth energ…⁶
##    <chr>        <lgl>   <lgl>    <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>  
##  1 A-Bomb       FALSE   TRUE     FALSE   FALSE   FALSE   TRUE    FALSE   FALSE  
##  2 Abe Sapien   TRUE    TRUE     FALSE   FALSE   TRUE    TRUE    FALSE   FALSE  
##  3 Angel        TRUE    TRUE     FALSE   FALSE   FALSE   TRUE    TRUE    FALSE  
##  4 Anti-Monitor FALSE   TRUE     FALSE   TRUE    FALSE   TRUE    FALSE   TRUE   
##  5 Anti-Venom   FALSE   TRUE     FALSE   FALSE   FALSE   TRUE    FALSE   FALSE  
##  6 Aquaman      TRUE    TRUE     FALSE   FALSE   TRUE    TRUE    TRUE    FALSE  
##  7 Arachne      TRUE    TRUE     FALSE   FALSE   FALSE   TRUE    FALSE   FALSE  
##  8 Archangel    TRUE    TRUE     FALSE   FALSE   FALSE   TRUE    FALSE   FALSE  
##  9 Ardina       TRUE    TRUE     FALSE   FALSE   TRUE    TRUE    FALSE   FALSE  
## 10 Ares         TRUE    TRUE     FALSE   FALSE   FALSE   TRUE    FALSE   FALSE  
## # … with 87 more rows, 159 more variables: flight <lgl>, danger_sense <lgl>,
## #   underwater_breathing <lgl>, marksmanship <lgl>, weapons_master <lgl>,
## #   power_augmentation <lgl>, animal_attributes <lgl>, longevity <lgl>,
## #   intelligence <lgl>, super_strength <lgl>, cryokinesis <lgl>,
## #   telepathy <lgl>, energy_armor <lgl>, energy_blasts <lgl>,
## #   duplication <lgl>, size_changing <lgl>, density_control <lgl>,
## #   stamina <lgl>, astral_travel <lgl>, audio_control <lgl>, dexterity <lgl>, …
```

## Your Favorite
15. Pick your favorite superhero and let's see their powers!

```r
superhero_powers
```

```
## # A tibble: 667 × 168
##    hero_names    agility accel…¹ lante…² dimen…³ cold_…⁴ durab…⁵ stealth energ…⁶
##    <chr>         <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>  
##  1 3-D Man       TRUE    FALSE   FALSE   FALSE   FALSE   FALSE   FALSE   FALSE  
##  2 A-Bomb        FALSE   TRUE    FALSE   FALSE   FALSE   TRUE    FALSE   FALSE  
##  3 Abe Sapien    TRUE    TRUE    FALSE   FALSE   TRUE    TRUE    FALSE   FALSE  
##  4 Abin Sur      FALSE   FALSE   TRUE    FALSE   FALSE   FALSE   FALSE   FALSE  
##  5 Abomination   FALSE   TRUE    FALSE   FALSE   FALSE   FALSE   FALSE   FALSE  
##  6 Abraxas       FALSE   FALSE   FALSE   TRUE    FALSE   FALSE   FALSE   FALSE  
##  7 Absorbing Man FALSE   FALSE   FALSE   FALSE   TRUE    TRUE    FALSE   TRUE   
##  8 Adam Monroe   FALSE   TRUE    FALSE   FALSE   FALSE   FALSE   FALSE   FALSE  
##  9 Adam Strange  FALSE   FALSE   FALSE   FALSE   FALSE   TRUE    TRUE    FALSE  
## 10 Agent Bob     FALSE   FALSE   FALSE   FALSE   FALSE   FALSE   TRUE    FALSE  
## # … with 657 more rows, 159 more variables: flight <lgl>, danger_sense <lgl>,
## #   underwater_breathing <lgl>, marksmanship <lgl>, weapons_master <lgl>,
## #   power_augmentation <lgl>, animal_attributes <lgl>, longevity <lgl>,
## #   intelligence <lgl>, super_strength <lgl>, cryokinesis <lgl>,
## #   telepathy <lgl>, energy_armor <lgl>, energy_blasts <lgl>,
## #   duplication <lgl>, size_changing <lgl>, density_control <lgl>,
## #   stamina <lgl>, astral_travel <lgl>, audio_control <lgl>, dexterity <lgl>, …
```


```r
superhero_powers %>% 
  filter(hero_names=="Harry Potter")
```

```
## # A tibble: 1 × 168
##   hero_…¹ agility accel…² lante…³ dimen…⁴ cold_…⁵ durab…⁶ stealth energ…⁷ flight
##   <chr>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl> 
## 1 Harry … TRUE    TRUE    FALSE   FALSE   FALSE   FALSE   TRUE    FALSE   FALSE 
## # … with 158 more variables: danger_sense <lgl>, underwater_breathing <lgl>,
## #   marksmanship <lgl>, weapons_master <lgl>, power_augmentation <lgl>,
## #   animal_attributes <lgl>, longevity <lgl>, intelligence <lgl>,
## #   super_strength <lgl>, cryokinesis <lgl>, telepathy <lgl>,
## #   energy_armor <lgl>, energy_blasts <lgl>, duplication <lgl>,
## #   size_changing <lgl>, density_control <lgl>, stamina <lgl>,
## #   astral_travel <lgl>, audio_control <lgl>, dexterity <lgl>, …
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
