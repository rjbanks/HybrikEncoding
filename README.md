# Hybrik Encoding

Base Hybrik Encoding Templates for encoding Atmos music

Example base JSON templates for Atmos music encoding jobs via Hybrik.

Fields with "{{xxxxx}}" require input before running Example: "name": "{{Name of Job}}" 

Example completed JSON
{
    "name": "Test Job",
    "task_tags": [
        ""
    ],
    "payload": {
        "elements": [
            {
                "uid": "source_file",
                "kind": "source",
                "payload": {
                    "kind": "asset_url",
                    "payload": {
                        "storage_provider": "s3",
                        "url": "s3://bucketname"
                    }
                }
            },
            {
                "uid": "dolby_audio_encoder_mp4",
                "kind": "dolby_audio_encoder",
                "task": {
                    "retry_method": "fail"
                },
                "payload": {
                    "location": {
                        "storage_provider": "s3",
                        "path": "s3://bucketname"
                    },
                    "targets": [
                        {
                            "file_pattern": "{source_basename}.mp4",
                            "existing_files": "replace",
                            "container": {
                                "kind": "mp4"
                            },
                            "audio": [
                                {
                                    "codec": "atmos",
                                    "bitrate_kb": 768,
                                    "filters": [
                                        {
                                            "audio_filter": {
                                                "kind": "loudness",
                                                "payload": {
                                                    "metering_mode": "1770-4",
                                                    "dialogue_intelligence": false,
                                                    "speech_threshold": 100
                                                }
                                            }
                                        },
                                        {
                                            "audio_filter": {
                                                "kind": "drc",
                                                "payload": {
                                                    "line_mode_drc_profile": "music_light",
                                                    "rf_mode_drc_profile": "music_light"
                                                }
                                            }
                                        },
                                        {
                                            "audio_filter": {
                                                "kind": "downmix",
                                                "payload": {
                                                    "loro_center_mix_level": -3,
                                                    "loro_surround_mix_level": -3,
                                                    "ltrt_center_mix_level": -3,
                                                    "ltrt_surround_mix_level": -3,
                                                    "preferred_downmix_mode": "loro"
                                                }
                                            }
                                        }
                                    ]
                                }
                            ]
                        }
                    ]
                }
            }
        ],
        "connections": [
            {
                "from": [
                    {
                        "element": "source_file"
                    }
                ],
                "to": {
                    "success": [
                        {
                            "element": "dolby_audio_encoder_mp4"
                        }
                    ]
                }
            }
        ]
    }
}


