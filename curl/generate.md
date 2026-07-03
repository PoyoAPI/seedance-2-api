# Seedance 2 cURL Examples

Use these requests to submit a Seedance 2 task and poll for the result.

## Generate

```bash
export POYO_API_KEY="YOUR_POYO_API_KEY_HERE"
export POYO_BASE_URL="https://api.poyo.ai"

curl --fail-with-body --request POST \
  --url "$POYO_BASE_URL/api/generate/submit" \
  --header "Authorization: Bearer $POYO_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{
    "model": "seedance-2",
    "input": {
      "prompt": "A short product teaser for a matte black electric kettle. Steam rises slowly, morning light enters from the window, camera pushes in, subtle kitchen ambience",
      "resolution": "720p",
      "duration": 8,
      "aspect_ratio": "16:9",
      "generate_audio": true
    }
  }'
```

Store the returned `data.task_id`, then poll:

```bash
curl --fail-with-body --request GET \
  --url "$POYO_BASE_URL/api/generate/status/task-unified-example" \
  --header "Authorization: Bearer $POYO_API_KEY"
```

## Fast Draft Request

Use `seedance-2-fast` when you want a quicker draft before spending more time on final video iterations.

```bash
curl --fail-with-body --request POST \
  --url "$POYO_BASE_URL/api/generate/submit" \
  --header "Authorization: Bearer $POYO_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{
    "model": "seedance-2-fast",
    "callback_url": "https://example.com/api/poyo/webhook",
    "input": {
      "prompt": "A quick product motion draft for a matte black electric kettle. Steam rises slowly, camera pushes in, subtle kitchen ambience",
      "resolution": "720p",
      "duration": 8,
      "aspect_ratio": "16:9",
      "generate_audio": true
    }
  }'
```

## Mini Draft Request

Use `seedance-2-mini` when you want a lower-cost draft loop before deciding which direction to polish.

```bash
curl --fail-with-body --request POST \
  --url "$POYO_BASE_URL/api/generate/submit" \
  --header "Authorization: Bearer $POYO_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{
    "model": "seedance-2-mini",
    "callback_url": "https://example.com/api/poyo/webhook",
    "input": {
      "prompt": "A lower-cost product video draft for a matte black electric kettle. Slow handheld push-in, natural morning light, no text",
      "resolution": "720p",
      "duration": 5,
      "aspect_ratio": "16:9",
      "generate_audio": false
    }
  }'
```

## Expected Submit Response

```json
{
  "code": 200,
  "data": {
    "task_id": "task-unified-example",
    "status": "not_started",
    "created_time": "2026-05-23T08:00:00"
  }
}
```

## Expected Status Response

```json
{
  "code": 200,
  "data": {
    "task_id": "task-unified-example",
    "status": "finished",
    "progress": 100,
    "files": [
      {
        "file_url": "https://storage.poyo.ai/generated/video.mp4",
        "file_type": "video"
      }
    ],
    "error_message": null
  }
}
```
