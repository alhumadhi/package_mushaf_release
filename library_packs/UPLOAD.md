# رفع حزم المكتبة إلى المستودع العام (إجراء يدوي)

الحزم (`*.bin`) و`manifest.json` تُستضاف في مستودع **عام**: `alhumadhi/package_mushaf_release` على الفرع `main` تحت `library_packs/`.
التطبيق يقرأ: `https://raw.githubusercontent.com/alhumadhi/package_mushaf_release/main/library_packs/manifest.json`
وكل حزمة من المجلد نفسه (`<slug>.bin`) ويتحقق من `sha256` قبل الحفظ.

## الخطوات (على جهازك، مرة واحدة)
```bash
git clone https://github.com/alhumadhi/package_mushaf_release.git
cd package_mushaf_release
mkdir -p library_packs
rsync -a --delete /path/to/Abdullah/library_packs/ library_packs/   # يتضمن UPLOAD.md؛ لا يضرّ
git add library_packs
git commit -m "مكتبة عند الطلب: حزم الكتب"
git push origin main
```
- كل ملف أقل من 6MB (حدّ GitHub العادي 100MB لكل ملف، والتوصية الكلية للمستودع ≲ 1GB) فلا حاجة لـ Git LFS.
- إن كان الدفع كبيرًا (> 100MB للدفعة الواحدة) ادفع على دفعات: `git add library_packs/a*.bin && git commit && git push` ثم البقية، **وادفع `manifest.json` أخيرًا** حتى لا يُعرض كتاب غير مرفوع بعد.
- بعد الرفع تحقق: `curl -sI https://raw.githubusercontent.com/alhumadhi/package_mushaf_release/main/library_packs/manifest.json` يعيد 200.
- راجع `LIBRARY_PACKS_README.md` لتوليد الحزم من جديد: `python3 tools/build_library_packs.py` (يصل فقط إلى raw.githubusercontent.com).

## صيغة manifest (الإصدار 2، متوافق خلفيًا)
حقول كل كتاب: `slug, title, author, deathYear, category, sizeBytes, sha256, chapters, source{name,url,license,originalPublisher,editor,versionUri,doi}`.
الكتب المقسَّمة تضيف حقولًا **اختيارية**: `group` (معرّف الكتاب الأم)، `groupTitle`، `part`، `parts`، `partTitle`؛ وكل جزء حزمة مستقلة `<group>-pNN.bin`.
العملاء القدامى يتجاهلون الحقول الإضافية ويعرضون كل جزء ككتاب بعنوان «… — الجزء k من n».
