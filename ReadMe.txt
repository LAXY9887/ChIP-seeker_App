※ 注意事項
1. Input bed 檔的 chr 要與 GTF 一致!!
2. 改 *.bed 檔案 Chr name, 取第一行 sed 改 成ChrX 輸出後手動貼上在 peaks.bed (可以用Excel改)
$ awk '{print $1}' beta-ES_peaksk.bed | sed 's/1/Chr1/g; s/2/Chr2/g; s/3/Chr3/g; s/4/Chr4/g; s/5/Chr5/g; s/mitochondria/ChrM/g; s/chloroplast/ChrC/g' > chrID_changed.txt

※ 使用方式
1. ./script/ChIPseeker_build_genome.R 建立 database
- 輸入 GTF 或 GFF3
- 輸出 *.sqlite database
* 若使用現成的 database 如 hg38(人) 或 mm10(鼠) 則不用

2. ./script/ChIPseeker.R 分析 peaks 分布、組成和相關基因
- 輸入1 *peaks.bed (MACS1.4的Peak檔); 
※ 格式: 5行 chr / start / end / peak_ID / 數字(enrichment或其他意義)
- 輸入2 *.sqlite database檔案
※ 或指定 'TxDb.Hsapiens.UCSC.hg38.knownGene' 'org.Hs.eg.db' 'EnsDb.Hsapiens.v86' 等 database
- 輸出 pdf檔案
- 設定:
# Input parameters
 peak.file <- "beta-ES_peaksk.bed" 					#MACS1.4 bed 檔 
 database <- "./result/txdb.TAIR10.sqlite"			#*.sqlite database檔案
 promoter_range = 3000								#設定promoter範圍
 output_path = "./result/"							#輸出路徑
 saved_name <- "beta-ES"							#輸出檔案名稱前綴
- 設定好之後執行全部
- 檢查 ./result/ 中輸出的 PDF 檔

3. ./script/ChIPseeker_compare.R 比較分析不同實驗 peaks 分布、組成和相關基因
- 輸入1 多個*peaks.bed (MACS1.4的Peak檔);
- 輸入2 *.sqlite database檔案
※ 或指定 'TxDb.Hsapiens.UCSC.hg38.knownGene' 'org.Hs.eg.db' 'EnsDb.Hsapiens.v86' 等 database
- 輸出 pdf檔案
- 設定:
# Input parameters
 peak.files <- c(
   "DMSO_peaksk.bed",
   "beta-ES_peaksk.bed"
 )											#多個MACS1.4 bed 檔, 以集合表示 
 database <- "./result/txdb.TAIR10.sqlite"	#*.sqlite database檔案
 promoter_range = 3000						#設定promoter範圍
 output_path = "./result/"					#輸出路徑
 saved_name <- "compare"					#輸出檔案名稱前綴(不要重複)