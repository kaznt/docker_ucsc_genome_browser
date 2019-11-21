### UCSC Genome Browser Docker Image

A minimal UCSC Genome Browser mirror.

http://genome.ucsc.edu/

### License
This is a Dockerized version of the UCSC Genome Browser source code. The license is the same as the UCSC Genome Browser itself. The source code and executables are freely available for academic, nonprofit and personal use. Commercial use requires purchase of a license with setup fee and annual payment. See https://genome-store.ucsc.edu/.

### Download
```shell
docker pull icebert/ucsc_genome_browser

docker pull icebert/ucsc_genome_browser_db
```

### Demo Run
```shell
docker run -d --name gbdb -p 3338:3306 icebert/ucsc_genome_browser_db

docker run -d --link gbdb:gbdb -p 8038:80 icebert/ucsc_genome_browser
```

### Run with local data files
Assume local data is going to be stored in /my/data/path

First copy the basic database files into /my/data/path from docker

```shell
docker run -d --name gbdb -p 3338:3306 icebert/ucsc_genome_browser_db

cd /my/data/path && docker cp gbdb:/data ./ && mv data/* ./ && rm -rf data

docker stop gbdb
```

Then put database files into /my/data/path. For example, mirror all the tracks of hg38 from ucsc genome browser

```shell
rm -rf /my/data/path/hg38

rsync -avP --delete --max-delete=20 rsync://hgdownload.soe.ucsc.edu/mysql/hg38 /my/data/path/
```

Finally start the database server and genome browser server

```shell
docker run -d --name gbdb -p 3338:3306 -v /my/data/path:/data icebert/ucsc_genome_browser_db

docker run -d --link gbdb:gbdb -p 8038:80 icebert/ucsc_genome_browser
```

### MySQL Access
The mysql server listens on port 3338. The default username for mysql is 'admin' with password 'admin'.

```shell
mysql -h 127.0.0.1 -P 3338 -u admin -p
```

In MySQL

Reffered http://hgdownload.soe.ucsc.edu/goldenPath/hgFixed/database/gtexInfo.*

Reffered http://hgdownload.soe.ucsc.edu/goldenPath/hgFixed/database/gtexTissue.*

on 191121

```shell
use hgFixed;

DROP TABLE IF EXISTS `gtexInfo`;
CREATE TABLE `gtexInfo` (
  `version` varchar(255) NOT NULL,
  `releaseDate` varchar(255) NOT NULL,
  `maxMedianScore` double NOT NULL,
  PRIMARY KEY (`version`)
) ENGINE=MyISAM DEFAULT CHARSET=latin1;
INSERT INTO `gtexInfo`(`version`, `releaseDate`, `maxMedianScore`) VALUES('V4', '2014-08-01', '178213');
INSERT INTO `gtexInfo`(`version`, `releaseDate`, `maxMedianScore`) VALUES('V6', '2015-10-01', '711778');

DROP TABLE IF EXISTS `gtexTissue`;
CREATE TABLE `gtexTissue` (
  `id` int(10) unsigned NOT NULL,
  `name` varchar(255) NOT NULL,
  `description` varchar(255) NOT NULL,
  `organ` varchar(255) NOT NULL,
  `color` int(10) unsigned NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM DEFAULT CHARSET=latin1;
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('0','adiposeSubcut','Adipose - Subcutaneous','Adipose Tissue','16753999');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('1','adiposeVisceral','Adipose - Visceral (Omentum)','Adipose Tissue','15636992');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('2','adrenalGland','Adrenal Gland','Adrenal Gland','9419919');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('3','arteryAorta','Artery - Aorta','Blood Vessel','9116770');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('4','arteryCoronary','Artery - Coronary','Blood Vessel','15624784');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('5','arteryTibial','Artery - Tibial','Blood Vessel','16711680');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('6','bladder','Bladder','Bladder','13481886');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('7','brainAmygdala','Brain - Amygdala','Brain','15658496');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('8','brainAnCinCortex','Brain - Anterior cingulate cortex (BA24)','Brain','15658496');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('9','brainCaudate','Brain - Caudate (basal ganglia)','Brain','15658496');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('10','brainCerebelHemi','Brain - Cerebellar Hemisphere','Brain','15658496');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('11','brainCerebellum','Brain - Cerebellum','Brain','15658496');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('12','brainCortex','Brain - Cortex','Brain','15658496');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('13','brainFrontCortex','Brain - Frontal Cortex (BA9)','Brain','15658496');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('14','brainHippocampus','Brain - Hippocampus','Brain','15658496');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('15','brainHypothalamus','Brain - Hypothalamus','Brain','15658496');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('16','brainNucAccumbens','Brain - Nucleus accumbens (basal ganglia)','Brain','15658496');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('17','brainPutamen','Brain - Putamen (basal ganglia)','Brain','15658496');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('18','brainSpinalcord','Brain - Spinal cord (cervical c-1)','Brain','15658496');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('19','brainSubstanNigra','Brain - Substantia nigra','Brain','15658496');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('20','breastMamTissue','Breast - Mammary Tissue','Breast','52685');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('21','xformedlymphocytes','Cells - EBV-transformed lymphocytes','Blood','15631086');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('22','xformedfibroblasts','Cells - Transformed fibroblasts','Skin','10141901');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('23','ectocervix','Cervix - Ectocervix','Cervix Uteri','15652306');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('24','endocervix','Cervix - Endocervix','Cervix Uteri','15652306');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('25','colonSigmoid','Colon - Sigmoid','Colon','13481886');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('26','colonTransverse','Colon - Transverse','Colon','15648145');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('27','esophagusJunction','Esophagus - Gastroesophageal Junction','Esophagus','9139029');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('28','esophagusMucosa','Esophagus - Mucosa','Esophagus','9139029');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('29','esophagusMuscular','Esophagus - Muscularis','Esophagus','13478525');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('30','fallopianTube','Fallopian Tube','Fallopian Tube','15652306');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('31','heartAtrialAppend','Heart - Atrial Appendage','Heart','11817677');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('32','heartLeftVentricl','Heart - Left Ventricle','Heart','8009611');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('33','kidneyCortex','Kidney - Cortex','Kidney','13481886');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('34','liver','Liver','Liver','13481886');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('35','lung','Lung','Lung','10145074');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('36','minorSalivGland','Minor Salivary Gland','Salivary Gland','13481886');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('37','muscleSkeletal','Muscle - Skeletal','Muscle','8021998');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('38','nerveTibial','Nerve - Tibial','Nerve','16766720');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('39','ovary','Ovary','Ovary','16758465');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('40','pancreas','Pancreas','Pancreas','13474589');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('41','pituitary','Pituitary','Pituitary','11857588');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('42','prostate','Prostate','Prostate','14277081');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('43','skinNotExposed','Skin - Not Sun Exposed (Suprapubic)','Skin','3825613');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('44','skinExposed','Skin - Sun Exposed (Lower leg)','Skin','2003199');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('45','smallIntestine','Small Intestine - Terminal Ileum','Small Intestine13481886');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('46','spleen','Spleen','Spleen','13481886');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('47','stomach','Stomach','Stomach','16765851');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('48','testis','Testis','Testis','10921638');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('49','thyroid','Thyroid','Thyroid','35653');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('50','uterus','Uterus','Uterus','15652306');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('51','vagina','Vagina','Vagina','15652306');
INSERT INTO `gtexTissue`(`id`, `name`, `description`,`organ`,`color`) VALUES('52','wholeBlood','Whole Blood','Blood','16711935');
```
