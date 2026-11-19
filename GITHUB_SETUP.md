# GitHub publication checklist

Run these commands locally after reviewing the project:

```bash
git init
git add .
git commit -m "Initial reproducible WattWise pipeline"
git branch -M main
git remote add origin https://github.com/<YOUR-USERNAME>/WattWise.git
git push -u origin main
git log --date=iso-strict --pretty=format:'%H | %cI | %s' > results/commit_history.txt
python reproducibility/capture_provenance.py
```

After meaningful changes, commit them separately with descriptive messages. `results/commit_history.txt` must be generated in the real repository; this package deliberately does not fabricate hashes or timestamps.
