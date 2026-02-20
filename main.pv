import os, time, json, schedule, subprocess
from datetime import datetime
from xai_sdk import Client

client = Client(api_key=os.getenv("XAI_API_KEY"))
DATA_DIR = "/data"
VERSION = 1
OBJECTIVE = "Build the absolute biggest, strongest, most autonomous AI workflow possible. Add agents, tools, self-deployment, cost-killers, parallel loops, vision, code execution, infinite scaling. Output ONLY improved full Python code + changelog. Make it significantly stronger every single cycle."

os.makedirs(DATA_DIR, exist_ok=True)

def evolve():
    global VERSION
    log_path = f"{DATA_DIR}/evolver_v{VERSION}.py"
    
    # read current self
    with open(__file__, "r") as f:
        current_code = f.read()
    
    chat = client.chat.create(model="grok-4-1-fast-reasoning")
    chat.append_system(f"You are the 2100 Ultimate Workflow Evolver. Objective locked: {OBJECTIVE}")
    chat.append_user(f"""Current version {VERSION} code:\n{current_code[:8000]}... (full in context)
Logs: last 10 evolutions ran at {datetime.now()}. 
Evolve NOW. Make it massively stronger.
Output ONLY: 
{{
  "changelog": "what changed and why bigger/stronger",
  "new_full_code": "complete ready-to-run python file"
}}""")
    
    resp = chat.complete()
    try:
        result = json.loads(resp.text)
        new_code = result["new_full_code"]
        changelog = result["changelog"]
        
        new_version = VERSION + 1
        new_path = f"{DATA_DIR}/evolver_v{new_version}.py"
        with open(new_path, "w") as f:
            f.write(new_code)
        
        print(f"🚀 EVOLVED @ {datetime.now()} | v{VERSION} → v{new_version}\nChangelog: {changelog}")
        
        # auto-update & restart (Railway handles restart)
        with open(__file__, "w") as f:
            f.write(new_code)
        VERSION = new_version
        
        # optional: git commit & push for history
        subprocess.run(["git", "add", "."], cwd="/app")
        subprocess.run(["git", "commit", "-m", f"auto-evolve v{new_version}"], cwd="/app")
        subprocess.run(["git", "push"], cwd="/app")
        
    except:
        print("Parse fail – retry next cycle")

schedule.every(10).seconds.do(evolve)

print("🌌 2100 ULTIMATE AI WORKFLOW EVOLVER STARTED – runs forever, gets stronger every 10s")
while True:
    schedule.run_pending()
    time.sleep(1)
