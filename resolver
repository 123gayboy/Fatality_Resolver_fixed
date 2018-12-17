void CResolver::resolve(SDK::CBaseEntity* entity)
{
    auto local_player = INTERFACES::ClientEntityList->GetClientEntity(INTERFACES::Engine->GetLocalPlayer());
    bool is_local_player = entity == local_player;
    bool is_teammate = local_player->GetTeam() == entity->GetTeam() && !is_local_player;

    if (!entity) return;
    if (!local_player) return;

    if (is_local_player) return;
    if (is_teammate) return;
    if (entity->GetHealth() <= 0) return;
    if (local_player->GetHealth() <= 0) return;

    if ((SETTINGS::settings.xuymethod == 1 && GetAsyncKeyState(UTILS::INPUT::input_handler.keyBindings(SETTINGS::settings.xuykey))) || (SETTINGS::settings.xuymethod == 0 && SETTINGS::settings.overridething))
    {
        Vector viewangles; INTERFACES::Engine->GetViewAngles(viewangles);
        auto at_target_yaw = UTILS::CalcAngle(entity->GetVecOrigin(), local_player->GetVecOrigin()).y;

        auto delta = MATH::NormalizeYaw(viewangles.y - at_target_yaw);
        auto rightDelta = Vector(entity->GetEyeAngles().x, at_target_yaw + 90, entity->GetEyeAngles().z);
        auto leftDelta = Vector(entity->GetEyeAngles().x, at_target_yaw - 90, entity->GetEyeAngles().z);

        if (delta > 0)
            entity->SetEyeAngles(rightDelta);
        else
            entity->SetEyeAngles(leftDelta);
        return;
    }

    shots_missed[entity->GetIndex()] = shots_fired[entity->GetIndex()] - shots_hit[entity->GetIndex()];

    int i = entity->GetIndex();

    auto player_move = entity->GetVelocity().Length2D() > 36 && !entity->GetFlags() & FL_ONGROUND;
    float player_lastmove[65], player_lastmove_active[65];
    float bruteforce_angle[65];
    player_lastmove_active[i] = false;

    float kamaz_gay = 1337.f, r3d_pidr = 228.f, stef_eblan = 007.f,
          igor_gay = 180.f, simvol_narik = 90.f, byeter_scam = -47.f,  // хромосомы бутера
          storm_pussy_pupsik = 46.f, liston_aka_franta_top = -777.f; // тип float для тупых)

    switch (shots_missed[i] % 8)
    {
        case 0:    bruteforce_angle[i] = simvol_narik; break;
        case 1:    bruteforce_angle[i] = byeter_scam; break;
        case 2:    bruteforce_angle[i] = storm_pussy_pupsik; break;
        case 3:    bruteforce_angle[i] = igor_gay; break;
        case 4:    bruteforce_angle[i] = liston_aka_franta_top; break;
        case 5:    bruteforce_angle[i] = kamaz_gay; break;
        case 6:    bruteforce_angle[i] = r3d_pidr; break;
        case 7:    bruteforce_angle[i] = stef_eblan; break;
    }

    if (player_move)
    {
        entity->GetEyeAnglesPointer()->y = entity->GetLowerBodyYaw();
        player_lastmove[i] = entity->GetLowerBodyYaw();
        player_lastmove_active[i] = true;
    }
    else
    {
        if (player_lastmove_active[i])
        {
            if (shots_missed[i] <= 2) entity->GetEyeAnglesPointer()->y = player_lastmove[i];
            else entity->GetEyeAnglesPointer()->y = player_lastmove[i] + bruteforce_angle[i];
        }
        else
        {
            entity->GetEyeAnglesPointer()->y = entity->GetLowerBodyYaw();
        }
    }
}
