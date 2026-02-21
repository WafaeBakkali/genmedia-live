#!/bin/bash
set -e

PROJECT_ID=${1:?"Usage: ./deploy.sh <PROJECT_ID> <REGION>"}
REGION=${2:-"us-central1"}
SERVICE_NAME="genmedia-live"

echo "Enabling required APIs..."
gcloud services enable run.googleapis.com aiplatform.googleapis.com --project=$PROJECT_ID

echo "Deploying to Cloud Run..."
gcloud run deploy $SERVICE_NAME --source . --region $REGION --project $PROJECT_ID --allow-unauthenticated --memory 1Gi --timeout 300

echo "Deployment complete!"
gcloud run services describe $SERVICE_NAME --region $REGION --project $PROJECT_ID --format='value(status.url)'