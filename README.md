# Duplicati Advanced Options — Khmer Explanation (Core + HTTP + Scripting + Reporting)

> ឯកសារនេះបកស្រាយ “Advanced Options” ក្នុង Duplicati ជាភាសាខ្មែរ ដើម្បីឲ្យអ្នកយល់ថា option មួយៗធ្វើអ្វី, ពេលណាគួរប្រើ, និងអ្វីដែលត្រូវប្រយ័ត្ន។  
> Advanced options គឺជាជម្រើសបន្ថែមពី Destination + Source folders ដើម្បីគ្រប់គ្រងល្បឿន/សុវត្ថិភាព/របាយការណ៍/ស្គ្រីប។ ប្រើដោយប្រុងប្រយ័ត្ន។

---

## 0) Advanced Options ជាអ្វី?

ពេល backup នៅ Duplicati អ្នកត្រូវបញ្ជាក់ **Destination (ទីតាំងផ្ទុក)** + **Source folders (ថតដើម)** ជាចម្បង។  
**Advanced options** ជួយគ្រប់គ្រង៖

- ល្បឿន / concurrency
- សុវត្ថិភាព (encryption / verification)
- ការរក្សាទុក version (retention)
- Logging / debug
- Reporting (Email/HTTP/XMPP)
- Run scripts before/after

> ⚠️ សម្រាប់ “Normal operation” មិនចាំបាច់ប្រើ Advanced options ច្រើនទេ—ប្រើតែអ្វីដែលត្រូវការ។

---

## 1) Core options (គ្រប់គ្រង behavior របស់ backup engine)

### allow-full-removal
**Option:** `--allow-full-removal=false`  
- **ធ្វើអ្វី?** អនុញ្ញាតឲ្យលុប fileset/version ចុងក្រោយបាន (default មិនអោយលុប ដើម្បីការពារកុំលុបទាំង repo ដោយច្រឡំ)  
- **ពេលប្រើ**៖ reset repository ឬ policy ពិសេស  
- **ប្រយ័ត្ន**៖ ប្រើខុសអាចលុប backup ទាំងមូលបាន

### allow-missing-source
**Option:** `--allow-missing-source=false`  
- **ធ្វើអ្វី?** ឲ្យ job បន្ត ទោះ source folder/drive ខ្លះបាត់ (offline)  
- **ពេលប្រើ**៖ external drive មិន mount / network share បាត់  
- **ប្រយ័ត្ន**៖ អាចធ្វើឲ្យ backup ខ្វះទិន្នន័យជាផ្នែកៗ (partial)

### allow-passphrase-change
**Option:** `--allow-passphrase-change=false`  
- **ធ្វើអ្វី?** អនុញ្ញាតឲ្យប្តូរ passphrase (សម្រាប់ operation ដែលអនុញ្ញាត)  
- **ពេលប្រើ**៖ គម្រោងប្តូរ encryption passphrase (advanced)  
- **ប្រយ័ត្ន**៖ docs បញ្ជាក់ថា option នេះ **មិនអនុញ្ញាត** សម្រាប់ backup/repair

### allow-sleep
**Option:** `--allow-sleep=false`  
- **ធ្វើអ្វី?** អនុញ្ញាតឲ្យ Windows/OSX sleep ខណៈ backup/restore កំពុងដំណើរការ  
- **ពេលប្រើ**៖ laptop policy  
- **ប្រយ័ត្ន**៖ sleep អាចធ្វើឲ្យ job fail/timeout

### all-versions
**Option:** `--all-versions=false`  
- **ធ្វើអ្វី?** ពេល search/list/restore បង្ហាញ version ទាំងអស់ (មិនមែនតែ latest)  
- **ពេលប្រើ**៖ រក file version ចាស់ៗ  
- **ប្រយ័ត្ន**៖ យឺតជាង

### asynchronous-concurrent-upload-limit
**Option:** `--asynchronous-concurrent-upload-limit=4`  
- **ធ្វើអ្វី?** ចំនួន upload ដែលរត់ស្របគ្នា (concurrent)  
- **ពេលប្រើ**៖ SSD/backend លឿន អាចទុក 4+  
- **ប្រយ័ត្ន**៖ external HDD គួរដាក់ `1` (HDD មិនសូវល្អ multiple writes)

### asynchronous-upload-folder
**Option:** `--asynchronous-upload-folder=C:\Users\User\AppData\Local\Temp\`  
- **ធ្វើអ្វី?** ថត temp សម្រាប់ volumes រង់ចាំ upload  
- **ពេលប្រើ**៖ C: temp តូច → ប្ដូរទៅ drive មាន space  
- **ប្រយ័ត្ន**៖ ត្រូវមាន free space និង permission

### asynchronous-upload-limit
**Option:** `--asynchronous-upload-limit=4`  
- **ធ្វើអ្វី?** កំណត់ចំនួន volumes “រង់ចាំ upload” អតិបរមា  
- **ពេលប្រើ**៖ កុំឲ្យ temp ពេញ  
- **ប្រយ័ត្ន**៖ ទាបពេក → backup យឺត

### auto-cleanup
**Option:** `--auto-cleanup=false`  
- **ធ្វើអ្វី?** សម្អាត partial files នៅ destination ពេល job interrupted  
- **ពេលប្រើ**៖ network/backend disconnect ញឹកញាប់  
- **ប្រយ័ត្ន**៖ បន្ថែម I/O ពេល cleanup

### auto-compact-interval
**Option:** `--auto-compact-interval=0m`  
- **ធ្វើអ្វី?** ចន្លោះពេលអប្បបរមា មុន auto-compact រត់ម្តងទៀត  
- **ពេលប្រើ**៖ compact យូរ → មិនចង់ឲ្យរត់ញឹកញាប់  
- **ប្រយ័ត្ន**៖ ត្រូវការ `--no-auto-compact=false` ដើម្បីមាន effect

### auto-update
**Option:** `--auto-update=false`  
- **ធ្វើអ្វី?** អោយ CLI auto update  
- **ពេលប្រើ**៖ lab/test environments  
- **ប្រយ័ត្ន**៖ production គួរគ្រប់គ្រង version ដោយដៃ

### auto-vacuum
**Option:** `--auto-vacuum=false`  
- **ធ្វើអ្វី?** អនុញ្ញាត Duplicati vacuum SQLite DB ដើម្បី reclaim space  
- **ពេលប្រើ**៖ DB ធំ/រ៉ាំរ៉ៃ  
- **ប្រយ័ត្ន**៖ vacuum អាចយូរ និងត្រូវការទំហំបន្ថែម

### auto-vacuum-interval
**Option:** `--auto-vacuum-interval=0m`  
- **ធ្វើអ្វី?** ចន្លោះពេលអប្បបរមារវាង vacuum  
- **ពេលប្រើ**៖ កុំ vacuum ញឹកញាប់  
- **ប្រយ័ត្ន**៖ ត្រូវការ `--auto-vacuum=true`

### backup-name
**Option:** `--backup-name=Duplicati.CommandLine`  
- **ធ្វើអ្វី?** ឈ្មោះ job (ចេញនៅ report/subject/scripts)  
- **ពេលប្រើ**៖ គួរដាក់ជានិច្ច  
- **ប្រយ័ត្ន**៖ គ្មាន

### backup-test-percentage
**Option:** `--backup-test-percentage=0`  
- **ធ្វើអ្វី?** បន្ទាប់ backup សាកល្បង verify remote files តាម %  
- **ពេលប្រើ**៖ data critical → ចង់ verify ច្រើន  
- **ប្រយ័ត្ន**៖ % ខ្ពស់ → time/bandwidth ច្រើន

### backup-test-samples
**Option:** `--backup-test-samples=1`  
- **ធ្វើអ្វី?** ចំនួន sample ក្នុងមួយ category (dblock/dindex/dlist) សម្រាប់ verify  
- **ពេលប្រើ**៖ tune verification  
- **ប្រយ័ត្ន**៖ 0 ឬ `--no-backend-verification` → មិន verify

### block-hash-algorithm
**Option:** `--block-hash-algorithm=SHA256`  
- **ធ្វើអ្វី?** hash algorithm សម្រាប់ blocks  
- **ពេលប្រើ**៖ advanced tuning/compliance  
- **ប្រយ័ត្ន**៖ កុំប្ដូរលើ repo ដែលមានរួចដោយគ្មានហេតុផលច្បាស់

### blocksize
**Option:** `--blocksize=100kb`  
- **ធ្វើអ្វី?** ទំហំ block (fragmentation)  
- **ពេលប្រើ**៖ tuning files ធំ vs file ច្រើនតូចៗ  
- **ប្រយ័ត្ន**៖ **ប្ដូរមិនបាន** បន្ទាប់ពី remote files បានបង្កើតរួច

### changed-files
**Option:** `--changed-files`  
- **ធ្វើអ្វី?** scan តែ file ដែល “ដឹងថាប្រែ” (ទាមទារ filesystem watcher)  
- **ពេលប្រើ**៖ dataset ធំ + watcher setup  
- **ប្រយ័ត្ន**៖ ប្រើដោយគ្មាន watcher → មិនបានអត្ថប្រយោជន៍

### check-filetime-only
**Option:** `--check-filetime-only=false`  
- **ធ្វើអ្វី?** detect changes ដោយ filetime តែប៉ុណ្ណោះ (មិនពិនិត្យ size/metadata)  
- **ពេលប្រើ**៖ scan យឺតខ្លាំង និង filetime ទុកចិត្តបាន  
- **ប្រយ័ត្ន**៖ app ខ្លះបំភាន់ timestamp → អាច miss changes

### compression-extension-file
**Option:** `--compression-extension-file=C:\Program Files\Duplicati 2\default_compressed_extensions.txt`  
- **ធ្វើអ្វី?** បញ្ជី extension ដែល “មិន compress” (zip មិនសន្សំ)  
- **ពេលប្រើ**៖ speed up + កាត់ CPU  
- **ប្រយ័ត្ន**៖ tuning; អាចប៉ះ file ដែល compress បាន

### compression-module
**Option:** `--compression-module=zip`  
- **ធ្វើអ្វី?** ជ្រើស compression module  
- **ពេលប្រើ**៖ repo ថ្មី/compatibility  
- **ប្រយ័ត្ន**៖ repo មានរួច អាច mixed modules តាម filename

### concurrency-block-hashers
**Option:** `--concurrency-block-hashers=2`  
- **ធ្វើអ្វី?** ចំនួន thread hashing blocks  
- **ពេលប្រើ**៖ CPU multi-core  
- **ប្រយ័ត្ន**៖ ខ្ពស់ពេក → CPU 100%

### concurrency-compressors
**Option:** `--concurrency-compressors=2`  
- **ធ្វើអ្វី?** ចំនួន thread compress  
- **ពេលប្រើ**៖ CPU ខ្លាំង  
- **ប្រយ័ត្ន**៖ CPU load

### concurrency-max-threads
**Option:** `--concurrency-max-threads=0`  
- **ធ្វើអ្វី?** thread max; 0 = auto balance  
- **ពេលប្រើ**៖ shared server → ចង់ limit  
- **ប្រយ័ត្ន**៖ limit ទាប → យឺត

### console-log-filter
**Option:** `--console-log-filter=...`  
- **ធ្វើអ្វី?** filter log tag include/exclude (pattern/regex)  
- **ពេលប្រើ**៖ console noisy  
- **ប្រយ័ត្ន**៖ filter ខុស → មើលមិនឃើញ error

### console-log-level
**Option:** `--console-log-level=Warning`  
- **ធ្វើអ្វី?** log level នៅ console  
- **ពេលប្រើ**៖ debug → Information/Retry  
- **ប្រយ័ត្ន**៖ logs ច្រើន

### control-files
**Option:** `--control-files=false`  
- **ធ្វើអ្វី?** use control files (rare/advanced)  
- **ពេលប្រើ**៖ only when you know you need it  
- **ប្រយ័ត្ន**៖ complexity

### dblock-size
**Option:** `--dblock-size=50mb`  
- **ធ្វើអ្វី?** ទំហំអតិបរមា dblock files  
- **ពេលប្រើ**៖ backend មាន limit per file  
- **ប្រយ័ត្ន**៖ តូចពេក → files ច្រើន

### dbpath
**Option:** `--dbpath=...`  
- **ធ្វើអ្វី?** path ទៅ local database cache  
- **ពេលប្រើ**៖ move DB to another drive  
- **ប្រយ័ត្ន**៖ DB បាត់/ខូច → operations យឺត/rebuild

### debug-output
**Option:** `--debug-output=false`  
- **ធ្វើអ្វី?** error message លម្អិតជាង  
- **ពេលប្រើ**៖ troubleshoot  
- **ប្រយ័ត្ន**៖ logs ច្រើន

### debug-retry-errors
**Option:** `--debug-retry-errors=false`  
- **ធ្វើអ្វី?** បង្ហាញ error រាល់ retry  
- **ពេលប្រើ**៖ network/back-end errors analysis  
- **ប្រយ័ត្ន**៖ logs ច្រើន

### default-filters
**Option:** `--default-filters` (Windows/OSX/Linux/All)  
- **ធ្វើអ្វី?** exclude sets ដោយ OS (system junk/temp)  
- **ពេលប្រើ**៖ general cleanup  
- **ប្រយ័ត្ន**៖ អាច exclude អ្វីដែលអ្នកចង់បាន

### deleted-files
**Option:** `--deleted-files=...`  
- **ធ្វើអ្វី?** បញ្ជី file ដែលបានលុប (ប្រើជាមួយ `--changed-files`)  
- **ពេលប្រើ**៖ watcher-based incremental  
- **ប្រយ័ត្ន**៖ គ្មាន `--changed-files` → ignore

### disable-autocreate-folder
**Option:** `--disable-autocreate-folder=false`  
- **ធ្វើអ្វី?** បិទ auto-create destination folder  
- **ពេលប្រើ**៖ strict policy  
- **ប្រយ័ត្ន**៖ folder មិនមាន → job fail

### disable-filepath-cache
**Option:** `--disable-filepath-cache=true`  
- **ធ្វើអ្វី?** កាត់ RAM ដោយមិន cache paths/timestamps  
- **ពេលប្រើ**៖ RAM តិច  
- **ប្រយ័ត្ន**៖ អាចយឺត

### disable-filetime-check
**Option:** `--disable-filetime-check=false`  
- **ធ្វើអ្វី?** មិនប្រើ last-write time ដើម្បី detect changes  
- **ពេលប្រើ**៖ app បំភាន់ timestamps  
- **ប្រយ័ត្ន**៖ scan យឺត

### disable-on-battery
**Option:** `--disable-on-battery=false`  
- **ធ្វើអ្វី?** scheduled backup មិនរត់បើប្រើ battery  
- **ពេលប្រើ**៖ laptop  
- **ប្រយ័ត្ន**៖ manual/CLI នៅតែរត់

### disable-module
**Option:** `--disable-module=module1,module2`  
- **ធ្វើអ្វី?** unload modules (sendmail/sendhttp/etc)  
- **ពេលប្រើ**៖ troubleshooting  
- **ប្រយ័ត្ន**៖ បិទ sendmail/sendhttp → report មិនដំណើរការ

### disable-piped-streaming
**Option:** `--disable-piped-streaming=false`  
- **ធ្វើអ្វី?** បិទ multithread piped streaming  
- **ពេលប្រើ**៖ backend issue with streaming  
- **ប្រយ័ត្ន**៖ speed ធ្លាក់

### disable-streaming-transfers
**Option:** `--disable-streaming-transfers=false`  
- **ធ្វើអ្វី?** បិទ streaming interface → progress/throttle មិនដំណើរការ  
- **ពេលប្រើ**៖ backend/proxy issues  
- **ប្រយ័ត្ន**៖ throttle មិន effective

### disable-synthetic-filelist
**Option:** `--disable-synthetic-filelist=false`  
- **ធ្វើអ្វី?** បិទ synthetic filelist creation ពេល previous backup incomplete  
- **ពេលប្រើ**៖ edge-case troubleshooting  
- **ប្រយ័ត្ន**៖ អាចប៉ះ consistency

### disable-time-tolerance
**Option:** `--disable-time-tolerance=false`  
- **ធ្វើអ្វី?** បិទ tolerance 1% (max 1h) ក្នុង time-based retention  
- **ពេលប្រើ**៖ strict pruning  
- **ប្រយ័ត្ន**៖ clock drift → delete មុន

### dont-compress-restore-paths
**Option:** `--dont-compress-restore-paths=false`  
- **ធ្វើអ្វី?** restore subset: keep original structure (avoid path shortening)  
- **ពេលប្រើ**៖ preserve structure  
- **ប្រយ័ត្ន**៖ paths អាចវែង

### dont-read-manifests
**Option:** `--dont-read-manifests=false`  
- **ធ្វើអ្វី?** មិនអាន manifests + មិន check hashes  
- **ពេលប្រើ**៖ disaster recovery only  
- **ប្រយ័ត្ន**៖ integrity risk

### dry-run
**Option:** `--dry-run=false`  
- **ធ្វើអ្វី?** សាកល្បង run ដោយមិនប្តូរ/មិន upload ពិត  
- **ពេលប្រើ**៖ test filters/config  
- **ប្រយ័ត្ន**៖ មិនបាន backup ពិត

### enable-module
**Option:** `--enable-module=module1,module2`  
- **ធ្វើអ្វី?** load modules  
- **ពេលប្រើ**៖ rare cases  
- **ប្រយ័ត្ន**៖ none

### encryption-module
**Option:** `--encryption-module=aes`  
- **ធ្វើអ្វី?** ជ្រើស encryption module  
- **ពេលប្រើ**៖ repo ថ្មី/compliance  
- **ប្រយ័ត្ន**៖ ប្ដូរកណ្ដាល repo អាចបង្ក complexity

### exclude
**Option:** `--exclude=pattern`  
- **ធ្វើអ្វី?** exclude files/folders matching pattern/regex  
- **ពេលប្រើ**៖ exclude temp/cache/iso  
- **ប្រយ័ត្ន**៖ exclude ខុស → បាត់ data

### exclude-empty-folders
**Option:** `--exclude-empty-folders=false`  
- **ធ្វើអ្វី?** មិន backup folder ទទេ  
- **ពេលប្រើ**៖ reduce noise  
- **ប្រយ័ត្ន**៖ structure អាចបាត់

### exclude-files-attributes
**Option:** `--exclude-files-attributes=Hidden,System,...`  
- **ធ្វើអ្វី?** exclude តាម attributes  
- **ពេលប្រើ**៖ Windows cleanup  
- **ប្រយ័ត្ន**៖ អាច exclude file សំខាន់

### file-hash-algorithm
**Option:** `--file-hash-algorithm=SHA256`  
- **ធ្វើអ្វី?** hash algorithm សម្រាប់ files  
- **ពេលប្រើ**៖ advanced tuning  
- **ប្រយ័ត្ន**៖ keep default unless needed

### file-read-buffer-size
**Option:** `--file-read-buffer-size=0kb`  
- **ធ្វើអ្វី?** buffer size ពេល read files  
- **ពេលប្រើ**៖ IO tuning  
- **ប្រយ័ត្ន**៖ setting ខុស → slow/RAM usage

### force-locale
**Option:** `--force-locale=...`  
- **ធ្វើអ្វី?** បង្ខំ locale/language output  
- **ពេលប្រើ**៖ want specific locale  
- **ប្រយ័ត្ន**៖ rare

### full-block-verification
**Option:** `--full-block-verification=false`  
- **ធ្វើអ្វី?** verify hash blocks ទាំងអស់  
- **ពេលប្រើ**៖ data critical + backend unreliable  
- **ប្រយ័ត្ន**៖ យឺតខ្លាំង

### full-remote-verification
**Option:** `--full-remote-verification=false`  
- **ធ្វើអ្វី?** decrypt+verify content of selected volumes  
- **ពេលប្រើ**៖ strengthen integrity check  
- **ប្រយ័ត្ន**៖ time/bandwidth ច្រើន

### full-result
**Option:** `--full-result=false`  
- **ធ្វើអ្វី?** output រួមទាំង filenames ទាំងអស់  
- **ពេលប្រើ**៖ audit/debug  
- **ប្រយ័ត្ន**៖ logs huge

### hardlink-policy (Linux/OSX)
**Option:** `--hardlink-policy=All`  
- **ធ្វើអ្វី?** របៀប handle hardlinks  
- **ពេលប្រើ**៖ Linux/OSX filesystems  
- **ប្រយ័ត្ន**៖ setting ខុស → duplication/restore quirks

### ignore-filenames
**Option:** `--ignore-filenames=.nobackup;...` (Windows uses `;`, Linux uses `:`)  
- **ធ្វើអ្វី?** បើ folder មាន marker file → exclude folder នោះ  
- **ពេលប្រើ**៖ user-friendly exclusion marker  
- **ប្រយ័ត្ន**៖ separator ត្រូវត្រឹមត្រូវ

### include
**Option:** `--include=pattern`  
- **ធ្វើអ្វី?** include តាម pattern/regex  
- **ពេលប្រើ**៖ whitelist backups (include only)  
- **ប្រយ័ត្ន**៖ include/exclude conflict → test

### index-file-policy
**Option:** `--index-file-policy=Full`  
- **ធ្វើអ្វី?** កម្រិតព័ត៌មានក្នុង index files (ជួយពេលគ្មាន local DB)  
- **ពេលប្រើ**៖ portable/DB might be missing  
- **ប្រយ័ត្ន**៖ Full → remote space ច្រើន

### keep-time
**Option:** `--keep-time=30D`  
- **ធ្វើអ្វី?** រក្សាទុក backups តាមពេលវេលា  
- **ពេលប្រើ**៖ retention simple  
- **ប្រយ័ត្ន**៖ មិនប្រើជាមួយ `--retention-policy`

### keep-versions
**Option:** `--keep-versions=10` (`-1` = keep all)  
- **ធ្វើអ្វី?** រក្សាទុកតាមចំនួន version  
- **ពេលប្រើ**៖ retention by count  
- **ប្រយ័ត្ន**៖ keep all → growth unlimited

### list-folder-contents
**Option:** `--list-folder-contents=false`  
- **ធ្វើអ្វី?** list only entries in a folder path (listing behavior)  
- **ពេលប្រើ**៖ quick browse  
- **ប្រយ័ត្ន**៖ not a full tree

### list-prefix-only
**Option:** `--list-prefix-only=false`  
- **ធ្វើអ្វី?** return only largest common prefix path  
- **ពេលប្រើ**៖ identify root structure  
- **ប្រយ័ត្ន**៖ not full listing

### list-sets-only
**Option:** `--list-sets-only=false`  
- **ធ្វើអ្វី?** list filesets/versions only (faster)  
- **ពេលប្រើ**៖ quickly see versions  
- **ប្រយ័ត្ន**៖ no filenames

### list-verify-uploads
**Option:** `--list-verify-uploads=false`  
- **ធ្វើអ្វី?** verify uploads by listing backend contents  
- **ពេលប្រើ**៖ backend consistency checks  
- **ប្រយ័ត្ន**៖ slow on large repos

### log-file
**Option:** `--log-file=C:\Logs\duplicati.log`  
- **ធ្វើអ្វី?** សរសេរ internal logs ទៅ file  
- **ពេលប្រើ**៖ troubleshooting best practice  
- **ប្រយ័ត្ន**៖ disk usage

### log-file-log-filter
**Option:** `--log-file-log-filter=...`  
- **ធ្វើអ្វី?** filter log-file output  
- **ពេលប្រើ**៖ focused logs  
- **ប្រយ័ត្ន**៖ miss errors if too strict

### log-file-log-level
**Option:** `--log-file-log-level=Warning`  
- **ធ្វើអ្វី?** log level for log file  
- **ពេលប្រើ**៖ debug → Information/Retry  
- **ប្រយ័ត្ន**៖ logs huge

### log-level
**Option:** `--log-level=Warning`  
- **ធ្វើអ្វី?** level for writing into `--log-file`  
- **Options:** Error, Warning, DryRun, Information, Retry, Profiling, ExplicitOnly  
- **ពេលប្រើ**៖ Information/Retry for troubleshooting  
- **ប្រយ័ត្ន**៖ high verbosity → big logs

### log-retention
**Option:** `--log-retention=30D`  
- **ធ្វើអ្វី?** purge log data from DB after X days  
- **ពេលប្រើ**៖ keep DB smaller  
- **ប្រយ័ត្ន**៖ old logs gone

### no-auto-compact
**Option:** `--no-auto-compact=false`  
- **ធ្វើអ្វី?** disable automatic compaction  
- **ពេលប្រើ**៖ compact too slow; do manual compaction  
- **ប្រយ័ត្ន**៖ wasted space increases

### no-backend-verification
**Option:** `--no-backend-verification=false`  
- **ធ្វើអ្វី?** skip compare local DB vs remote filelist on startup  
- **ពេលប្រើ**៖ remote listing broken/unavailable  
- **ប្រយ័ត្ន**៖ less integrity assurance

### no-connection-reuse
**Option:** `--no-connection-reuse=false`  
- **ធ្វើអ្វី?** do not reuse connections (login each operation)  
- **ពេលប្រើ**៖ proxy/backend issue with keep-alive  
- **ប្រយ័ត្ន**៖ slower

### no-encryption
**Option:** `--no-encryption=false`  
- **ធ្វើអ្វី?** store backups unencrypted  
- **ពេលប្រើ**៖ local trusted disk only  
- **ប្រយ័ត្ន**៖ security risk if disk lost

### no-local-blocks
**Option:** `--no-local-blocks=false`  
- **ធ្វើអ្វី?** skip using local blocks optimization  
- **ពេលប្រើ**៖ troubleshooting correctness  
- **ប្រយ័ត្ន**៖ may download more

### no-local-db
**Option:** `--no-local-db=false`  
- **ធ្វើអ្វី?** skip local DB for list/restore (slower) to verify remote truth  
- **ពេលប្រើ**៖ DB suspected corrupt  
- **ប្រយ័ត្ន**៖ slower

### number-of-retries
**Option:** `--number-of-retries=5`  
- **ធ្វើអ្វី?** retry count on failures  
- **ពេលប្រើ**៖ unstable network  
- **ប្រយ័ត្ន**៖ more retries → longer jobs

### overwrite (restore)
**Option:** `--overwrite=false`  
- **ធ្វើអ្វី?** overwrite existing target files when restoring  
- **ពេលប្រើ**៖ restore to same folder  
- **ប្រយ័ត្ន**៖ can overwrite newer files

### parameters-file
**Option:** `--parameters-file=C:\Duplicati\options.txt`  
- **ធ្វើអ្វី?** store options in a text file (`--option=value` per line)  
- **ពេលប្រើ**៖ complex configs, easier management  
- **ប្រយ័ត្ន**៖ filters cannot be defined both in file + commandline

### passphrase
**Option:** `--passphrase=...`  
- **ធ្វើអ្វី?** passphrase for encrypting volumes  
- **ពេលប្រើ**៖ encryption enabled (recommended)  
- **ប្រយ័ត្ន**៖ lose passphrase = cannot restore

### patch-with-local-blocks
**Option:** `--patch-with-local-blocks=false`  
- **ធ្វើអ្វី?** search local machine for blocks to reduce downloads  
- **ពេលប្រើ**៖ local has related data  
- **ប្រយ័ត្ន**៖ slow scan

### prefix
**Option:** `--prefix=duplicati`  
- **ធ្វើអ្វី?** prefix remote filenames so multiple backups can share same folder  
- **ពេលប្រើ**៖ multiple jobs same destination  
- **ប្រយ័ត្ន**៖ prefix cannot contain `-` hyphen

### quiet-console
**Option:** `--quiet-console=false`  
- **ធ្វើអ្វី?** suppress console progress; rely on logs  
- **ពេលប្រើ**៖ background scheduled runs  
- **ប្រយ័ត្ន**៖ troubleshooting needs log-file

### quota-size
**Option:** `--quota-size=...`  
- **ធ្វើអ្វី?** set known max backend size when backend can’t report correctly  
- **ពេលប្រើ**៖ specific backends  
- **ប្រយ័ត្ន**៖ wrong size → wrong decisions

### repair-only-paths
**Option:** `--repair-only-paths=false`  
- **ធ្វើអ្វី?** build searchable DB with paths only  
- **ពេលប្រើ**៖ quick search of what’s backed up  
- **ប្រយ័ត្ន**៖ DB cannot be used to restore full data

### restore-path
**Option:** `--restore-path=D:\RestoreHere`  
- **ធ្វើអ្វី?** restore into another folder (default is original)  
- **ពេលប្រើ**៖ sandbox restore/test  
- **ប្រយ័ត្ន**៖ ensure space

### restore-permissions
**Option:** `--restore-permissions=false`  
- **ធ្វើអ្វី?** restore ACL/permissions  
- **ពេលប្រើ**៖ Linux servers where perms matter  
- **ប្រយ័ត្ន**៖ can lock you out if ACL wrong

### restore-symlink-metadata
**Option:** `--restore-symlink-metadata=false`  
- **ធ្វើអ្វី?** apply metadata to symlink itself  
- **ពេលប្រើ**៖ special symlink setups  
- **ប្រយ័ត្ន**៖ rare

### retention-policy
**Option:** `--retention-policy=7D:0s,3M:1D,10Y:2M`  
- **ធ្វើអ្វី?** smart retention: keep all for short period, then keep fewer long-term  
- **ពេលប្រើ**៖ long-term retention without huge growth  
- **ប្រយ័ត្ន**៖ mutually exclusive with `--keep-time` and `--keep-versions`

### retry-delay
**Option:** `--retry-delay=10s`  
- **ធ្វើអ្វី?** wait before retry  
- **ពេលប្រើ**៖ intermittent network drops  
- **ប្រយ័ត្ន**៖ large delay → longer jobs

### skip-file-hash-checks
**Option:** `--skip-file-hash-checks=false`  
- **ធ្វើអ្វី?** allow proceed even if volume hash mismatch  
- **ពេលប្រើ**៖ emergency/disaster recovery only  
- **ប្រយ័ត្ន**៖ risk corrupted data

### skip-files-larger-than
**Option:** `--skip-files-larger-than=5GB`  
- **ធ្វើអ្វី?** exclude files larger than size  
- **ពេលប្រើ**៖ avoid huge VM/ISO  
- **ប្រយ័ត្ន**៖ may skip important data

### skip-metadata
**Option:** `--skip-metadata=false`  
- **ធ្វើអ្វី?** don’t store metadata (timestamps/attrs)  
- **ពេលប្រើ**៖ you don’t care about metadata; speed up  
- **ប្រយ័ត្ន**៖ restored files may lack timestamps/attrs

### skip-restore-verification
**Option:** `--skip-restore-verification=false`  
- **ធ្វើអ្វី?** skip hashing verification after restore  
- **ពេលប្រើ**៖ fastest restore  
- **ប្រយ័ត្ន**៖ integrity not checked

### small-file-max-count
**Option:** `--small-file-max-count=20`  
- **ធ្វើអ្វី?** group small volumes to reduce tiny-file spam  
- **ពេលប្រើ**៖ many tiny files  
- **ប្រយ័ត្ន**៖ tuning

### small-file-size
**Option:** `--small-file-size=...`  
- **ធ្វើអ្វី?** tolerance used when considering compaction  
- **ពេលប្រើ**៖ advanced tuning  
- **ប្រយ័ត្ន**៖ wrong values affect compact behavior

### snapshot-policy
**Option:** `--snapshot-policy=off|auto|on|required`  
- **ធ្វើអ្វី?** control snapshots (Windows VSS / Linux LVM) for locked files  
- **ពេលប្រើ**៖ backup locked files → `auto` (recommended)  
- **ប្រយ័ត្ន**៖ `required` can fail backups if VSS fails (needs admin)

### store-metadata
**Option:** `--store-metadata=true`  
- **ធ្វើអ្វី?** store metadata (timestamps/attrs)  
- **ពេលប្រើ**៖ most cases keep true  
- **ប្រយ័ត្ន**៖ small overhead

### symlink-policy
**Option:** `--symlink-policy=Store|Ignore|Follow`  
- **ធ្វើអ្វី?**
  - `Store`: record symlink as link  
  - `Ignore`: ignore all symlinks  
  - `Follow`: backup target content as normal file  
- **ពេលប្រើ**៖ projects with symlinks  
- **ប្រយ័ត្ន**៖ `Follow` may duplicate data

### synchronous-upload
**Option:** `--synchronous-upload=false`  
- **ធ្វើអ្វី?** false: upload while scanning (faster); true: wait per volume (sequential)  
- **ពេលប្រើ**៖ backend unstable (sometimes sequential helps)  
- **ប្រយ័ត្ន**៖ true is usually slower

### tempdir
**Option:** `--tempdir=C:\Temp`  
- **ធ្វើអ្វី?** temp folder for Duplicati volumes/cache (SQLite temp still uses system temp)  
- **ពេលប្រើ**៖ move temp away from C:  
- **ប្រយ័ត្ន**៖ needs space + permission

### thread-priority
**Option:** `--thread-priority=normal`  
- **ធ្វើអ្វី?** process thread priority (CPU scheduling)  
- **ពេលប្រើ**៖ work PC → lower; backup server → normal  
- **ប្រយ័ត្ន**៖ high priority can lag system

### threshold
**Option:** `--threshold=25`  
- **ធ្វើអ្វី?** % wasted space before reclaiming via compaction  
- **ពេលប្រើ**៖ tune compact aggressiveness  
- **ប្រយ័ត្ន**៖ too low → compact often (slow)

### throttle-download / throttle-upload
**Option:** `--throttle-download=0kb` / `--throttle-upload=0kb`  
- **ធ្វើអ្វី?** limit bandwidth  
- **ពេលប្រើ**៖ shared network  
- **ប្រយ័ត្ន**៖ too low → backup slow

### time
**Option:** `--time=now` (e.g. `--time=-2M`)  
- **ធ្វើអ្វី?** select which backup time to list/restore from  
- **ពេលប្រើ**៖ restore old versions  
- **ប្រយ័ត្ន**៖ syntax must be valid

### unittest-mode
**Option:** `--unittest-mode=false`  
- **ធ្វើអ្វី?** testing mode; no automatic fixes  
- **ពេលប្រើ**៖ developers/testing only  
- **ប្រយ័ត្ន**៖ not for daily backups

### upload-unchanged-backups
**Option:** `--upload-unchanged-backups=false`  
- **ធ្វើអ្វី?** upload backupset even if no changes (proof-of-run)  
- **ពេលប្រើ**៖ compliance/audit wants proof  
- **ប្រយ័ត្ន**៖ more remote sets

### upload-verification-file
**Option:** `--upload-verification-file=false`  
- **ធ្វើអ្វី?** upload unencrypted verification file with size+SHA256 of remote files  
- **ពេលប្រើ**៖ independent verification  
- **ប្រយ័ត្ន**៖ file not encrypted (metadata exposure)

### use-background-io-priority
**Option:** `--use-background-io-priority=false`  
- **ធ្វើអ្វី?** lower IO priority (less intrusive)  
- **ពេលប្រើ**៖ user PC while working  
- **ប្រយ័ត្ន**៖ slower

### use-block-cache
**Option:** `--use-block-cache=false`  
- **ធ្វើអ្វី?** keep in-memory block table for faster lookups  
- **ពេលប្រើ**៖ RAM plenty + big backups  
- **ប្រយ័ត្ន**៖ higher RAM usage

### usn-policy (Windows)
**Option:** `--usn-policy=off|auto|on|required`  
- **ធ្វើអ្វី?** use NTFS USN journal for faster listing/scanning  
- **ពេលប្រើ**៖ NTFS + admin → `auto` (recommended)  
- **ប្រយ័ត្ន**៖ `required` fails if USN not available

### verbose
**Option:** `--verbose=false`  
- **ធ្វើអ្វី?** output line per file processed  
- **ពេលប្រើ**៖ debug include/exclude behavior  
- **ប្រយ័ត្ន**៖ logs huge

### version
**Option:** `--version=0,2-4,7`  
- **ធ្វើអ្វី?** select specific version index for list/restore  
- **ពេលប្រើ**៖ restore exact set  
- **ប្រយ័ត្ន**៖ must exist

### vss-exclude-writers
**Option:** `--vss-exclude-writers=GUID1;GUID2`  
- **ធ្វើអ្វី?** exclude problematic VSS writers (Windows)  
- **ពេលប្រើ**៖ VSS errors tied to a writer  
- **ប្រយ័ត្ន**៖ excluding wrong writer may affect snapshot correctness

### vss-use-mapping
**Option:** `--vss-use-mapping=false`  
- **ធ្វើអ្វី?** map VSS snapshots to drive letter (legacy workaround)  
- **ពេលប្រើ**៖ rare/legacy OS  
- **ប្រយ័ត្ន**៖ not common today

---

## 2) HTTP options (កែ behavior HTTP requests)

### disable-expect100-continue
**Option:** `--disable-expect100-continue=false`  
- **ធ្វើអ្វី?** បិទ header `Expect: 100-Continue`  
- **ពេលប្រើ**៖ server ចេញ error 417  
- **ប្រយ័ត្ន**៖ usually leave default

### disable-nagling
**Option:** `--disable-nagling=false`  
- **ធ្វើអ្វី?** បិទ Nagle algorithm (small packet optimization)  
- **ពេលប្រើ**៖ latency issues  
- **ប្រយ័ត្ន**៖ rare

### accept-specified-ssl-hash
**Option:** `--accept-specified-ssl-hash=SHA1HEX`  
- **ធ្វើអ្វី?** accept SSL cert hash ជាក់លាក់ (self-signed)  
- **ពេលប្រើ**៖ internal server self-signed cert  
- **ប្រយ័ត្ន**៖ safer than accept-any

### accept-any-ssl-certificate
**Option:** `--accept-any-ssl-certificate=false`  
- **ធ្វើអ្វី?** accept any SSL cert even invalid  
- **ពេលប្រើ**៖ temporary troubleshooting only  
- **ប្រយ័ត្ន**៖ security risk (MITM)

### oauth-url
**Option:** `--oauth-url=https://duplicati-oauth-handler.appspot.com/refresh`  
- **ធ្វើអ្វី?** alternate OAuth refresh URL  
- **ពេលប្រើ**៖ you host your own handler  
- **ប្រយ័ត្ន**៖ rare

### allowed-ssl-versions
**Option:** `--allowed-ssl-versions=SystemDefault,Ssl3,Tls` (example)  
- **ធ្វើអ្វី?** control allowed SSL/TLS versions  
- **ពេលប្រើ**៖ handshake issues / tighten security  
- **ប្រយ័ត្ន**៖ too strict → connect fails

### http-operation-timeout
**Option:** `--http-operation-timeout=...`  
- **ធ្វើអ្វី?** timeout for entire HTTP operation  
- **ពេលប្រើ**៖ slow backend  
- **ប្រយ័ត្ន**៖ too low → failures

### http-readwrite-timeout
**Option:** `--http-readwrite-timeout=...`  
- **ធ្វើអ្វី?** max time between activity on a connection (stall detection)  
- **ពេលប្រើ**៖ stalls  
- **ប្រយ័ត្ន**៖ too aggressive → false timeouts

### http-enable-buffering
**Option:** `--http-enable-buffering=false`  
- **ធ្វើអ្វី?** enable HTTP buffering  
- **ពេលប្រើ**៖ specific cases only  
- **ប្រយ័ត្ន**៖ docs warns about possible memory leaks

---

## 3) Scripting options (Run scripts)

### run-script-before
**Option:** `--run-script-before=C:\scripts\before.bat`  
- **ធ្វើអ្វី?** រត់ script មុន operation ចាប់ផ្តើម (blocking)  
- **ពេលប្រើ**៖ mount drive, stop service, pre-checks  
- **ប្រយ័ត្ន**៖ script hang → backup delayed

### run-script-after
**Option:** `--run-script-after=C:\scripts\after.bat`  
- **ធ្វើអ្វី?** រត់ script ក្រោយ operation ចប់; result written to stdout  
- **ពេលប្រើ**៖ custom notifications, cleanup  
- **ប្រយ័ត្ន**៖ script errors are separate from backup engine unless you act on them

### run-script-before-required
**Option:** `--run-script-before-required=C:\scripts\required.bat`  
- **ធ្វើអ្វី?** script មុន operation; non-zero/timeout → abort operation  
- **ពេលប្រើ**៖ must-have checks (e.g., destination reachable)  
- **ប្រយ័ត្ន**៖ script issue = backup won’t run

### run-script-timeout
**Option:** `--run-script-timeout=60s`  
- **ធ្វើអ្វី?** max time script allowed; if exceeded, operation continues and script output ignored  
- **ពេលប្រើ**៖ prevent long blocking  
- **ប្រយ័ត្ន**៖ timeout does not necessarily kill script

---

## 4) Reporting options (HTTP / Email / XMPP)

### HTTP reporting

#### send-http-url (deprecated)
- **ធ្វើអ្វី?** option ចាស់; ឥឡូវប្រើ `--send-http-form-urls` ឬ `--send-http-json-urls`

#### send-http-form-urls
**Option:** `--send-http-form-urls=https://...;https://...`  
- **ធ្វើអ្វី?** POST form-encoded data ទៅ URL(s) (multiple separated by `;`)  
- **ពេលប្រើ**៖ Telegram API, simple webhooks  
- **ប្រយ័ត្ន**៖ endpoint must accept form data

#### send-http-json-urls
**Option:** `--send-http-json-urls=https://...;https://...`  
- **ធ្វើអ្វី?** POST JSON data ទៅ URL(s)  
- **ពេលប្រើ**៖ webhook servers expecting JSON  
- **ប្រយ័ត្ន**៖ endpoint must accept JSON

#### send-http-message
**Option:** `--send-http-message=...`  
- **ធ្វើអ្វី?** message template (or filename). Tokens supported:  
  - `%OPERATIONNAME%` (Backup/Restore/...)  
  - `%REMOTEURL%` destination  
  - `%LOCALPATH%` source path  
  - `%PARSEDRESULT%` (Success/Warning/Error for backup)  
  - `%RESULT%` (result)  
  - `%value%` (any option values)  
- **ពេលប្រើ**៖ custom content in webhook message  
- **ប្រយ័ត្ន**៖ formatting/newlines in UI may need care

#### send-http-message-parameter-name
**Option:** `--send-http-message-parameter-name=message`  
- **ធ្វើអ្វី?** field name for the message parameter  
- **ពេលប្រើ**៖ Telegram usually needs `text` → set `text`  
- **ប្រយ័ត្ន**៖ wrong field → webhook receives nothing

#### send-http-extra-parameters
**Option:** `--send-http-extra-parameters=chat_id=123&parse_mode=HTML`  
- **ធ្វើអ្វី?** add extra parameters to request  
- **ពេលប្រើ**៖ Telegram requires chat_id  
- **ប្រយ័ត្ន**៖ encode special characters properly

#### send-http-level
**Option:** `--send-http-level=All`  
- **ធ្វើអ្វី?** when to send: Success, Warning, Error, Fatal, All  
- **ពេលប្រើ**៖ reduce spam → `Error,Warning`  
- **ប្រយ័ត្ន**៖ All = many notifications

#### send-http-any-operation
**Option:** `--send-http-any-operation=false`  
- **ធ្វើអ្វី?** default sends only after Backup; true sends for all operations  
- **ពេលប្រើ**៖ want notifications on restore/test/compact  
- **ប្រយ័ត្ន**៖ more notifications

#### send-http-result-output-format
**Option:** `--send-http-result-output-format=Duplicati|Json`  
- **ធ្វើអ្វី?** output formatting of results payload  
- **ពេលប្រើ**៖ receiver expects JSON  
- **ប្រយ័ត្ន**៖ receiver parsing must match

---

### Email reporting (SMTP)

#### send-mail-to
**Option:** `--send-mail-to=viputh54@gmail.com`  
- **ធ្វើអ្វី?** recipients (required)  
- **ពេលប្រើ**៖ email reporting  
- **ប្រយ័ត្ន**៖ multiple recipients separated by commas

#### send-mail-from
**Option:** `--send-mail-from=no-reply`  
- **ធ្វើអ្វី?** sender address/name  
- **ពេលប្រើ**៖ consistent sender  
- **ប្រយ័ត្ន**៖ some SMTP servers enforce valid sender domains

#### send-mail-subject
**Option:** `--send-mail-subject=Duplicati %OPERATIONNAME% report for %backup-name%`  
- **ធ្វើអ្វី?** subject template (tokens supported)  
- **ពេលប្រើ**៖ identify job quickly

#### send-mail-body
**Option:** `--send-mail-body=...` (or a filename)  
- **ធ្វើអ្វី?** body template (tokens supported as above)  
- **ពេលប្រើ**៖ full report format  
- **ប្រយ័ត្ន**៖ newlines formatting in UI needs care

#### send-mail-url
**Option:** `--send-mail-url=smtp://smtp.gmail.com:587/?starttls=always`  
- **ធ្វើអ្វី?** SMTP server URL (supports SSL/STARTTLS)  
- **ពេលប្រើ**៖ recommended to set explicitly  
- **ប្រយ័ត្ន**៖ Gmail usually needs App Password (if 2FA)

#### send-mail-username / send-mail-password
**Options:** `--send-mail-username=...` / `--send-mail-password=...`  
- **ធ្វើអ្វី?** SMTP authentication  
- **ពេលប្រើ**៖ most SMTP servers  
- **ប្រយ័ត្ន**៖ avoid real account password; use app password where available

#### send-mail-level
**Option:** `--send-mail-level=All`  
- **ធ្វើអ្វី?** send conditions (Success/Warning/Error/Fatal/All)  
- **ពេលប្រើ**៖ reduce spam → Error,Warning  
- **ប្រយ័ត្ន**៖ All = many emails

#### send-mail-any-operation
**Option:** `--send-mail-any-operation=false`  
- **ធ្វើអ្វី?** default send only after Backup; true send for all operations  
- **ពេលប្រើ**៖ notifications on restore/test  
- **ប្រយ័ត្ន**៖ more emails

#### send-mail-result-output-format
**Option:** `--send-mail-result-output-format=Duplicati|Json`  
- **ធ្វើអ្វី?** output formatting of results in email  
- **ពេលប្រើ**៖ machine-parsed results  
- **ប្រយ័ត្ន**៖ JSON email less readable

---

### XMPP reporting (rare)

#### send-xmpp-to / send-xmpp-username / send-xmpp-password
- **ធ្វើអ្វី?** credentials + recipients for XMPP (Jabber)  
- **ពេលប្រើ**៖ orgs using XMPP  
- **ប្រយ័ត្ន**៖ requires server/account configuration

#### send-xmpp-message
- **ធ្វើអ្វី?** message template like send-http-message  
- **tokens**៖ `%OPERATIONNAME% %REMOTEURL% %LOCALPATH% %PARSEDRESULT% %RESULT%`

#### send-xmpp-level / send-xmpp-any-operation / send-xmpp-result-output-format
- **ធ្វើអ្វី?** same meaning as email/http levels and any-operation flags

---

## 5) Practical “Recommended set” (សម្រាប់ local backup + email + telegram)

### Core (recommended)
```text
--backup-name=OfficeData
--snapshot-policy=auto
--allow-missing-source=true
--asynchronous-concurrent-upload-limit=1
--keep-time=30D
--auto-cleanup=true
--log-level=Information
--log-file=C:\Logs\duplicati.log
